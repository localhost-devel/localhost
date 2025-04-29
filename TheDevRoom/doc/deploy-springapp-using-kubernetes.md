# 🚀 Spring Boot + MongoDB Kubernetes Deployment Guide

This guide helps you deploy a **Spring Boot application** with **MongoDB** on a **Kubernetes cluster**. You will learn how to build the Docker image, push it to Docker Hub, and deploy your application to Kubernetes.

---

## 🛠 Prerequisites

Ensure the following tools are installed on your system:

- 🐳 **Docker** – For container image building and pushing.
- ☕ **Java & Maven** – To build the Spring Boot application.
- ☸️ **Kubernetes Cluster** – You can use Minikube for local development.
    - 📘 [Minikube Setup Guide](https://github.com/localhost-devel/localhost/blob/master/TheDevRoom/kubernetes-setup/setup_minikube.md)
    - 📘 [MicroK8s Setup Guide](https://github.com/localhost-devel/localhost/blob/master/TheDevRoom/kubernetes-setup/setup_microk8s.md)
- 🔧 **kubectl** – Command-line tool to manage Kubernetes resources.
- 📦 **Spring Boot Application Repository** – Clone the application repository from GitHub:
  - [springboot-mongodb-app](https://github.com/localhost-devel/springboot-mongodb-app.git)

---

## 🔨 1. Build the Spring Boot Project

```bash
mvn clean package
```

> This command compiles your Spring Boot project and generates a `.jar` file inside the `target/` directory.

---

## 🛠 2. Build the Docker Image

```bash
docker build -t thedevroom/spring-boot-mongo-application:1 .
```

> This builds a Docker image using your `Dockerfile` and tags it as `thedevroom/spring-boot-mongo-application:1`.

---

## 🔐 3. Login to Docker Hub

```bash
docker login -u thedevroom
```

> Enter your Docker Hub password when prompted to authenticate.

---

## 📦 4. Push the Docker Image to Docker Hub

```bash
docker push thedevroom/spring-boot-mongo-application:1
```

> This uploads your built Docker image to Docker Hub, making it accessible for Kubernetes to pull.

---

## 🚀 5. Deploy the Application to Kubernetes

```bash
kubectl apply -f AppConfig.yaml
```

> Deploys both the Spring Boot application and MongoDB services to your Kubernetes cluster using the provided manifest.

---

## 📊 6. Monitor Pod Status

```bash
watch kubectl get pods -n app
```

> Continuously monitors the pod statuses in the `app` namespace, helping you track the deployment process.

---

## 🌐 7. Access the Spring Boot Application

### Option 1: Get NodePort manually

```bash
kubectl get svc -n app
```

> Find the NodePort assigned to your `springapp-service`, then access it via:

```
http://<NodeIP>:<NodePort>
```

### Option 2: Open via Minikube (for local development)

```bash
minikube service springapp-service -n app
```

> This command opens the service URL directly in your browser (only works with Minikube).

---

## 🔎 8. Access the Pod (for logs/debugging)

### Step 1: Get the Pod Name

```bash
kubectl get pods -n app
```

### Step 2: Access the Pod shell

```bash
kubectl exec -it <springapp-pod-name> -n app -- /bin/sh
```

Example:

```bash
kubectl exec -it springapp-6f55c47d9c-abc12 -n app -- /bin/sh
```

> This lets you interact with your Spring Boot app container directly for troubleshooting.

---

## 📁 Kubernetes Configuration (`AppConfig.yaml`)

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: app
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: springapp
  namespace: app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: springapp
  template:
    metadata:
      labels:
        app: springapp
    spec:
      containers:
      - name: springapp-container
        image: thedevroom/spring-boot-mongo-application:1
        ports:
        - containerPort: 8080
        env:
        - name: MONGO_DB_HOSTNAME
          value: db-service
        - name: MONGO_DB_USERNAME
          value: dbuser
        - name: MONGO_DB_PASSWORD
          value: dbadmin@123
---
apiVersion: v1
kind: Service
metadata:
  name: springapp-service
  namespace: app
spec:
  selector:
    app: springapp
  ports:
  - port: 80
    targetPort: 8080
  type: NodePort
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
  namespace: app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongo
        image: mongo
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USERNAME
          value: dbuser
        - name: MONGO_INITDB_ROOT_PASSWORD
          value: dbadmin@123
---
apiVersion: v1
kind: Service
metadata:
  name: db-service
  namespace: app
spec:
  selector:
    app: mongo
  ports:
  - port: 27017
    targetPort: 27017
```

> This configuration defines the `springapp` and `mongo` deployments, their associated services, and environment variables.

---

## 🧹 9. Cleanup Kubernetes Resources

To remove all the resources created during the deployment:

```bash
kubectl delete namespace app
```

> This deletes the entire `app` namespace, cleaning up the deployments, pods, and services created within it.

---

## ✅ Final Notes

- Ensure your Spring Boot app exposes port `8080` for communication with the MongoDB service.
- In **production environments**, consider storing sensitive credentials securely using **Kubernetes Secrets** rather than embedding them in plain text.
- For rapid testing, use `kubectl port-forward` to directly forward traffic to your application instead of relying on NodePort.
- This guide is optimized for **local development or testing clusters** and assumes you are using Minikube or another local Kubernetes solution.

---

## ✅ Summary

This Kubernetes deployment guide allows you to:

- **Build and push** your Docker image to Docker Hub.
- **Deploy** your Spring Boot and MongoDB application to Kubernetes.
- **Monitor and debug** deployments via `kubectl`.
- **Access** your services via NodePort or Minikube for local testing.
- **Cleanup** resources when you're done to maintain a clean environment.

This setup is ideal for **local development** and **test environments**, ensuring you have an easy way to manage your Spring Boot + MongoDB stack in Kubernetes.

#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---
