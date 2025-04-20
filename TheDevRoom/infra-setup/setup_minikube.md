
# 🐳 Minikube Kubernetes Cluster Setup Guide

This guide provides step-by-step instructions for setting up a local Kubernetes cluster using **Minikube**.

---

## 🔧 Prerequisites

- A hypervisor (e.g., VirtualBox, HyperKit, KVM, Hyper-V, Docker)
- kubectl installed ([Install guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/))

- Minikube installed ([Install guide](https://minikube.sigs.k8s.io/docs/start/))
```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

---

## 🚀 Start Minikube

```bash
minikube start --driver=docker
```

Replace `docker` with your preferred driver if needed.

---

## ✅ Check Cluster Status

```bash
minikube status
alias kubectl='minikube kubectl --'
echo "alias kubectl='minikube kubectl --'" >> ~/.bashrc
source ~/.bashrc
kubectl get nodes
kubectl get ns
```

---

## 🌐 Access Kubernetes Dashboard

```bash
minikube dashboard
```

This opens the Kubernetes dashboard in your default browser.

---

## 📂 Interact with Minikube Filesystem

```bash
minikube ssh
```

To access the Minikube VM shell.

---

## 📦 Deploy an Application

```bash
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
minikube service hello-minikube
```

---

## 🧹 Stop and Delete Cluster

```bash
minikube stop
minikube delete
```

---

## 📚 Resources

- 🔗 [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- 🔗 [Kubernetes Documentation](https://kubernetes.io/docs/)

