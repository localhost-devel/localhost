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

### Port-forward the Argo Server

```bash
kubectl port-forward svc/argo-server -n argo 2746:2746
```

Then access the Argo Workflows UI at:

```
http://localhost:2746
```

---

## 🔐 Authentication

By default, authentication is disabled in the Helm chart. If you wish to enable it, check the [Argo Workflows SSO guide](https://argo-workflows.readthedocs.io/en/stable/sso/).

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

---

## 📚 References

- [Argo Workflows GitHub](https://github.com/argoproj/argo-workflows)
- [Helm Chart Repo](https://github.com/argoproj/argo-helm)
- [Official Documentation](https://argo-workflows.readthedocs.io/)
