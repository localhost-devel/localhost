# 🛠️ AWX Operator Installation using Helm Chart

[AWX](https://github.com/ansible/awx) is the open-source version of Ansible Tower. The **AWX Operator** helps manage the deployment and lifecycle of AWX in Kubernetes clusters.

This guide walks you through installing and accessing AWX using the **Helm chart for the AWX Operator**, with access through `ClusterIP` using `kubectl port-forward`.

---

## 📋 Prerequisites

- A running Kubernetes cluster (v1.21+)
- `kubectl` configured to communicate with your cluster
- Helm v3.6 or newer
- Optional: `git` if you want to clone the Helm repo

---

## 🚀 Installation Steps

### 1. Add the Helm Repository

```bash
helm repo add awx-operator https://ansible-community.github.io/awx-operator-helm/
helm repo update
```

---

### 2. Create Namespace for AWX (Optional)

```bash
kubectl create namespace awx
```

---

### 3. Install the AWX Operator

```bash
helm install awx-operator awx-operator/awx-operator \
  --namespace awx \
  --create-namespace
```

> 🔧 You can override chart values using `-f values.yaml`.

---

### 4. Verify Operator Deployment

```bash
kubectl get pods -n awx
```

Expected output:

```
NAME                                                  READY   STATUS    RESTARTS   AGE
awx-operator-controller-manager-xxxxxxxxxx-xxxxx      2/2     Running   0          1m
```

---

## ⚙️ Deploy AWX Instance

Apply the following AWX custom resource to trigger the actual AWX deployment:

```bash
kubectl apply -f - <<EOF
apiVersion: awx.ansible.com/v1beta1
kind: AWX
metadata:
  name: awx
  namespace: awx
spec:
  service_type: ClusterIP
  ingress_type: none
  hostname: awx.example.com
  replicas: 1
EOF
```

> You may change `service_type` to `LoadBalancer` if you're deploying in a cloud environment and want external access.

---

### 5. Monitor the AWX Deployment

```bash
kubectl get pods -n awx
```

Wait until all pods are in the `Running` state.

---

### 6. Optional: Run Migrations (if needed)

```bash
kubectl exec -it -n awx <awx-web-pod-name> --container=awx-web -- awx-manage migrate --noinput
```

---

## 🌐 Accessing the AWX Web UI (via ClusterIP + Port Forward)

If using `ClusterIP`:

```bash
kubectl port-forward svc/awx-service -n awx 8043:80
```

Then open your browser and visit:

```
http://localhost:8043
```

If using `LoadBalancer`, get the external IP:

```bash
kubectl get svc -n awx
```

---

## 🔐 Admin Login Credentials

Get the default `admin` password:

```bash
kubectl get secret awx-admin-password -n awx -o jsonpath="{.data.password}" | base64 --decode && echo
```

- **Username**: `admin`
- **Password**: *(output from command above)*

---

## 📦 Uninstalling AWX

### Delete the AWX instance:

```bash
kubectl delete awx awx -n awx
```

### Uninstall the AWX Operator:

```bash
helm uninstall awx-operator -n awx
```

### Delete the Namespace (Optional):

```bash
kubectl delete namespace awx
```

---

## 📚 References

- [AWX Operator GitHub](https://github.com/ansible/awx-operator)
- [AWX Project](https://github.com/ansible/awx)
- [Helm Chart Repo](https://ansible-community.github.io/awx-operator-helm/)
- [AWX Installation Docs](https://github.com/ansible/awx/blob/devel/INSTALL.md)
