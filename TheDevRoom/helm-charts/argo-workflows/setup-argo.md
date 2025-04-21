# ⚙️ Argo Workflows Installation using Helm

[Argo Workflows](https://github.com/argoproj/argo-workflows) is an open-source container-native workflow engine for orchestrating parallel jobs on Kubernetes.

This guide walks you through installing **Argo Workflows** using the **official Helm chart**, and accessing the Argo UI using `ClusterIP` and `kubectl port-forward`.

---

## 📋 Prerequisites

- A running Kubernetes cluster (v1.21+)
- `kubectl` configured to communicate with your cluster
- Helm v3.6 or newer
- Optional: `git` for cloning chart repo

---

## 🚀 Installation Steps

### 1. Add the Argo Helm Repository

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

---

### 2. Create Namespace for Argo Workflows

```bash
kubectl create namespace argo
```

---

### 3. Install Argo Workflows via Helm

```bash
helm install argo argo/argo-workflows \
  --namespace argo \
  --create-namespace
```

> 🔧 Use `-f values.yaml` to override configuration as needed.

---

### 4. Verify the Deployment

```bash
kubectl get pods -n argo
```

Expected output:

```
NAME                                   READY   STATUS    RESTARTS   AGE
argo-workflows-server-xxxxx-xxxxx      1/1     Running   0          1m
workflow-controller-xxxxx-xxxxx        1/1     Running   0          1m
```

---

## 🌐 Accessing the Argo Workflows UI (via ClusterIP + Port Forward)

### Setup foundation resources for argo
```bash
kubectl apply -f foundation.yaml
```

### Verify foundation resources:
```bash
kubectl get clusterrole -n argo | grep argo-sa-role
kubectl get clusterrolebinding -n argo | grep argo-sa-role-binding
kubectl get sa -n argo | grep argo-sa
kubectl get secret -n argo | grep argo-sa.service-account-token
```

#### Get Token
```bash
ARGO_TOKEN="Bearer $(kubectl get secret argo-sa.service-account-token -n argo -o=jsonpath='{.data.token}' | base64 --decode)"
echo $ARGO_TOKEN
```
Note: This token is used to authenticate Argo Workflows with the Kubernetes API server. It is important to understand that the token is associated with a service account, not with the Argo Workflows itself. As a result, even if the Argo Workflows pods or deployments are restarted, the service account—and its associated access permissions—will remain unchanged, ensuring consistent access control.

### Port-forward the Argo Server

```bash
kubectl port-forward svc/argo-argo-workflows-server -n argo 2746:2746
```

### Access Argo UI
Access the Argo Workflows UI using the following localhost URL, and use the previously generated token to authenticate with Argo Workflows:

[Argo Workflows UI](http://localhost:2746)

---

## 🔐 Authentication

By default, authentication is disabled in the Helm chart. If you wish to enable it, check the [Argo Workflows Documentation](https://argoproj.github.io/workflows/).

For local development or testing, the default access is open via `localhost`.

---

## 📦 Uninstalling Argo Workflows

### Delete the Argo Workflows deployment:

```bash
helm uninstall argo -n argo
```

### Delete the Namespace (Optional):

```bash
kubectl delete namespace argo
```

### Delete foundation resources:
```bash
kubectl delete -f foundation.yaml
```
---

## 📚 References

- [Argo Workflows GitHub](https://github.com/argoproj/argo-workflows)
- [Helm Chart Repo](https://github.com/argoproj/argo-helm)
- [Official Documentation](https://argo-workflows.readthedocs.io/)
