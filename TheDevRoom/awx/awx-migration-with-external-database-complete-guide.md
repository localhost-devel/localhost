# AWX Migration to External Database (AWS RDS) - Generalized Guide

## Overview

This document provides a comprehensive, step-by-step guide to migrating AWX from its internal PostgreSQL database to an external PostgreSQL instance hosted on AWS RDS. The process includes setting up a sample AWX environment, backing up existing data, restoring it to an external database, and reconfiguring AWX to use the new database.

## Prerequisites

Ensure the following before beginning the migration:

- A functional Kubernetes cluster.
- Helm installed and configured.
- Access to an external PostgreSQL database (AWS RDS).
- `kubectl` configured to access the Kubernetes cluster.
- `psql` client installed to interact with PostgreSQL databases.

## Migration Steps

### 1. Setup AWX in Sample Namespace Using Helm

Install the AWX operator and AWX instance using Helm in a `sample` namespace:

```bash
helm install -n sample --create-namespace awx . -f values.yaml
```

Monitor the pod status:

```bash
watch kubectl get pods -n sample
```

### 2. Retrieve AWX Admin Password and Service URL

Get the AWX admin password and service details:

```bash
kubectl -n sample get secret awx-admin-password -o jsonpath="{.data.password}" | base64 --decode; echo
kubectl get svc -n sample
```

### 3. Access AWX Web Interface

- Open the AWX Web UI using the retrieved service URL.
- Login using the admin credentials.
- Create sample projects, job templates, and inventories.
- Run a test job to verify the AWX instance functionality.

### 4. Setup External PostgreSQL Database

Connect to your AWS RDS instance:

```bash
psql -h <rds-endpoint> -p 5432 -d postgres -U <username>
```

Create the AWX-specific database and user:

```bash
CREATE DATABASE awxdb;
CREATE USER <username> WITH PASSWORD '<password>';
CREATE ROLE <username> WITH LOGIN PASSWORD '<password>';
GRANT rds_superuser TO <username>;
ALTER USER <username> CREATEDB;
GRANT ALL PRIVILEGES ON DATABASE awxdb TO <username>;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO <username>;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO <username>;
GRANT ALL PRIVILEGES ON ALL FUNCTIONS IN SCHEMA public TO <username>;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON TABLES TO <username>;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON SEQUENCES TO <username>;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT ALL PRIVILEGES ON FUNCTIONS TO <username>;
```

### 5. Backup Existing AWX Database

Scale down the AWX pods to avoid write conflicts:

```bash
kubectl scale deployment awx-task -n sample --replicas=0
kubectl scale deployment awx-web -n sample --replicas=0
```

Backup the AWX database:

```bash
kubectl get pods -n sample | grep postgres
kubectl exec -it <postgres-pod-name> -n sample -- pg_dump -U postgres \
--clean \
--if-exists \
--no-owner \
awx > awx_backup.sql
```

Verify the backup file:

```bash
ls -la awx_backup.sql
cat awx_backup.sql
head awx_backup.sql
```

### 6. Restore Data to External Database

Restore the database dump to the external PostgreSQL instance:

```bash
psql -h <rds-endpoint> -U <username> -d awxdb < awx_backup.sql
```

Validate restoration:

```bash
psql -h <rds-endpoint> -U <username> -d awxdb -c "SELECT count(*) FROM auth_user;"
```

### 7. Update AWX Configuration

Modify `values.yaml` to configure AWX with the external PostgreSQL database:

```yaml
AWX:
  # Other existing configuration...

  # Configurations for external PostgreSQL instance
  postgres:
    enabled: true
```

### 8. Update AWX PostgreSQL Configuration Secret

You can update the PostgreSQL configuration in one of the following ways:

**Option 1: Update Secret File Directly**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: awx-postgres-configuration
  namespace: sample
  annotations:
    meta.helm.sh/release-name: awx
    meta.helm.sh/release-namespace: sample
  labels:
    app.kubernetes.io/managed-by: Helm
type: Opaque
stringData:
  host: <rds-endpoint>
  port: "5432"
  database: awxdb
  username: <username>
  password: <password>
  type: unmanaged
```

**Option 2: Configure via values.yaml** (Recommended)

```yaml
AWX:
  # Other existing configuration...

  postgres:
    enabled: true
    host: <rds-endpoint>
    port: "5432"
    dbName: "awxdb"
    username: "<username>"
    password: "<password>"
    sslmode: "prefer"
    type: "unmanaged"
```

### 9. Upgrade AWX with External Database Configuration

Apply the updated configuration:

```bash
helm upgrade awx . -f values.yaml -n sample
```

Monitor pod status:

```bash
watch kubectl get pods -n sample
```

Retrieve the AWX admin password:

```bash
kubectl -n sample get secret awx-admin-password -o jsonpath="{.data.password}" | base64 --decode; echo
```

Access the AWX Web UI and validate login and functionality.

### 10. Verify the Migration

Check AWX pod statuses:

```bash
kubectl get pods -n sample
```

Verify database connectivity:

```bash
psql -h <rds-endpoint> -U <username> -d awxdb -c "\dt"
```

Perform data validation checks:

```bash
# Check user count
psql -h <rds-endpoint> -U <username> -d awxdb -c "SELECT count(*) FROM auth_user;"

# Check projects count
psql -h <rds-endpoint> -U <username> -d awxdb -c "SELECT count(*) FROM main_project;"

# Check job templates count
psql -h <rds-endpoint> -U <username> -d awxdb -c "SELECT count(*) FROM main_jobtemplate;"
```

Check AWX pod logs:

```bash
# Web pod logs
kubectl logs -f deployment/awx-web -n sample

# Task pod logs
kubectl logs -f deployment/awx-task -n sample
```

### 11. Cleanup and Validate Helm Deployment

Uninstall AWX and clean up resources:

```bash
helm uninstall awx -n sample
kubectl get all -n sample
kubectl delete namespace sample
```

Confirm deletion:

```bash
kubectl get all -n sample
```

Reinstall AWX using the external database configuration:

```bash
helm install awx . -f values.yaml -n sample --create-namespace
watch kubectl get pods -n sample
```

Login to the AWX web interface and verify connectivity and functionality.

## Conclusion

By following this detailed procedure, you can successfully migrate your AWX instance from an internal database to an external AWS RDS PostgreSQL instance. This method ensures minimal downtime and maintains data integrity throughout the migration process.

