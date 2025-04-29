# 🐳 MicroK8s Kubernetes Setup Guide

This guide helps you install and configure **MicroK8s** — a lightweight, production-grade Kubernetes distribution that runs locally.

---

## 🔧 Installation Steps

### 1. Install MicroK8s (Kubernetes v1.32)

```bash
sudo snap install microk8s --classic --channel=1.32
```

### 2. Add Your User to the MicroK8s Group

```bash
sudo usermod -a -G microk8s $USER
```

### 3. Create Kubernetes Config Directory

```bash
mkdir -p ~/.kube
chmod 0700 ~/.kube
```

### 4. Apply Group Changes

```bash
su - $USER
```

Re-login to apply group permissions.

---

## ✅ Check MicroK8s Status

```bash
microk8s status --wait-ready
```

Waits until all core Kubernetes services are ready.

---

## 📦 kubectl via MicroK8s

### 1. Check Cluster Nodes

```bash
microk8s kubectl get nodes
```

### 2. Set kubectl Alias

```bash
alias kubectl='microk8s kubectl'
```

To make it persistent, add it to your shell config (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
echo "alias kubectl='microk8s kubectl'" >> ~/.bashrc
source ~/.bashrc
```

---

## 🚀 Common kubectl Commands

```bash
kubectl get nodes        # View cluster nodes
kubectl get svc          # List all services
kubectl get ns           # List all namespaces
```

---

## 🔌 Optional: Enable MicroK8s Add-ons

```bash
microk8s enable dns ingress dashboard registry
```

---

## 🧹 Uninstall MicroK8s

```bash
sudo snap remove microk8s
```

---

## 📚 Resources

- 🔗 [MicroK8s Docs](https://microk8s.io/docs)
- 🔗 [Kubernetes Official Docs](https://kubernetes.io/docs/)

#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---
