# Amazon RDS PostgreSQL Setup and EKS Network/Security Configuration

## Overview
This document provides a standardized approach to setting up an Amazon RDS PostgreSQL database along with the necessary network and security configurations to ensure seamless connectivity from an Amazon EKS cluster or other compute resources within AWS.

## Components
The infrastructure stack includes:

- **Amazon RDS PostgreSQL** database instance
- **Security Groups** to control inbound and outbound traffic
- **AWS Secrets Manager** for managing database credentials
- **AWS KMS** (Key Management Service) for encrypting database storage and backups

## Network and Security Configuration

### Security Group Configuration
1. Create or modify a security group associated with the RDS instance.
2. Add inbound rules to allow traffic from EKS cluster nodes or associated compute resources.

| Setting  | Value |
|----------|-------|
| Protocol | TCP   |
| Port     | 5432  |
| Source   | EKS Node Security Group ID (or CIDR Block, if necessary) |

> **Note:**  
> Ensure that outbound rules allow response traffic. By default, security groups allow all outbound traffic.

### VPC Configuration
RDS and compute resources (EKS cluster nodes, EC2 instances) must have proper network connectivity. This can be achieved through:

- **Same VPC** (recommended for simplicity and performance)
- **VPC Peering** (for different VPCs)
- **AWS Transit Gateway** (for more complex, multi-VPC or multi-account architectures)

> **Important:**  
> Ensure appropriate route tables and NACLs (Network ACLs) are configured to allow traffic between resources.

## Database Credentials Management
- Store database credentials securely using **AWS Secrets Manager**.
- Define fine-grained access policies (IAM) to allow only authorized applications or services to retrieve secrets.

## Database Encryption
- Enable **storage encryption** using an **AWS KMS** customer-managed key (CMK) during RDS instance creation.
- Ensure backups, snapshots, and replicas are encrypted.

---

## Database Connection Testing

### Prerequisites
Before initiating a connection test:

- Confirm that the **RDS instance status** is **Available**.
- Ensure a **PostgreSQL client** (e.g., `psql`) is installed within the EKS pods, EC2 instances, or bastion hosts.
- Validate that security groups and VPC connectivity are correctly configured.
- Collect the following connection parameters:
    - **RDS Endpoint**
    - **Port** (default: 5432)
    - **Database Name**
    - **Username** and **Password** (from Secrets Manager or manually)

### Connection Validation Steps
From an EKS pod, EC2 instance, or bastion host, run:

```bash
psql -h <rds-endpoint> -p 5432 -d <database-name> -U <username>
```
You will be prompted for the password if not using IAM authentication.

> **Tip:**  
> If using Secrets Manager, you can automate retrieval of credentials with AWS SDKs or the AWS CLI.

---

## Planned Enhancements

### Enable IAM Authentication

- **Enable IAM Database Authentication** on the RDS PostgreSQL instance.
- **Attach IAM roles** to EKS pods (using IAM Roles for Service Accounts - IRSA) or EC2 instances to allow IAM-based authentication.
- **Grant RDS permissions** to the IAM roles/users using PostgreSQL `rds_iam` role mappings.

### Role-Based Access Control (RBAC)

- Implement fine-grained access controls within PostgreSQL using standard roles and permissions.
- Combine AWS IAM with database-level RBAC for secure and scalable access management.

---

## References

- [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/index.html)
- [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/index.html)
- [AWS Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
- [IAM Database Authentication for PostgreSQL and MySQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.html)
- [IAM Roles for Service Accounts (IRSA)](https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html)



#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---