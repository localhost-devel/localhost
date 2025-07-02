
# 🧰 Jenkins on Kubernetes Setup Guide

This guide walks you through deploying **Jenkins** on a Kubernetes cluster for CI/CD pipelines in a DevOps environment.

---

## 🔧 Prerequisites

- A running Kubernetes cluster (e.g., Minikube, MicroK8s)
- `kubectl` installed and configured
- Docker installed (for building container images)

---

## 🗂️ Step 1: Create Jenkins Namespace

```bash
kubectl apply -f namespace.yaml
```

---

## 👤 Step 2: Create a Service Account

```bash
kubectl apply -f serviceAccount.yaml
```

---

## 📡 Step 3: Verify Kubernetes Nodes

```bash
kubectl get nodes -o wide
```

---

## 💾 Step 4: Create Persistent Volume

Edit `volume.yaml` to include a specific node name from the previous step, then apply it:

```bash
kubectl apply -f volume.yaml
```

---

## 🚀 Step 5: Deploy Jenkins

```bash
kubectl apply -f deployment.yaml
```

---

## 📊 Step 6: Monitor Jenkins Deployment

```bash
watch kubectl get all -n jenkins
```
Wait until the Jenkins pod status is Running and the READY column shows 1/1, indicating the container is fully up and operational.

---

## 🗄️ Step 7: (Optional) Inspect Mounted Volumes

Inside the pod:

```bash
ls /mnt
```

---

## 🌐 Step 8: Expose Jenkins Service

```bash
kubectl apply -f service.yaml
```

---

## 🌍 Step 9: Get Node IP

```bash
kubectl get nodes -o wide
```

Use the external/internal IP and access Jenkins at:

```text
http://<node-ip>:32000
```

---

## 🔑 Step 10: Retrieve Admin Credentials
📝 Run the following command to check Jenkins pod logs and retrieve the initial admin password:

```bash
POD_NAME=$(kubectl get pods -n jenkins -l app=jenkins-server -o jsonpath="{.items[0].metadata.name}")

if [ -z "$POD_NAME" ]; then
  echo "❌ Jenkins pod not found. Make sure it is deployed and labeled correctly."
else
  kubectl logs "$POD_NAME" -n jenkins
fi

```

---

## 🧩 Step 11: Install Plugins & Start CI/CD

Once logged in, install required plugins and configure Jenkins pipelines as needed.

---

## 🧹 Step 12: Cleanup Jenkins Resources

```bash
kubectl delete ns jenkins
kubectl delete storageclass jenkins-storage
kubectl delete pv jenkins-pv-volume
```

---

## 📚 Resources

- 🔗 [Jenkins on Kubernetes Docs](https://www.jenkins.io/doc/book/installing/kubernetes/)
- 🔗 [Kubernetes Documentation](https://kubernetes.io/docs/)

---

#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)  
- 📞 Contact: +91 9999999999
