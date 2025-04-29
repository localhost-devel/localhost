# ⚙️ Manual Docker Setup for Spring Boot + MongoDB

Run your Spring Boot application with MongoDB using only Docker commands — no Docker Compose or Kubernetes needed.

---

## 🛠 Prerequisites

Ensure the following tools are installed on your system:

- 🐳 [Docker](https://docs.docker.com/get-docker/)
- ☕ Java (JDK 8 or later)
- 🛠️ [Maven](https://maven.apache.org/install.html)
- 📦 **Spring Boot Application Repository** – Clone the application repository from GitHub:
    - [springboot-mongodb-app](https://github.com/localhost-devel/springboot-mongodb-app.git)


---

## 🔨 1. Build the Spring Boot Project

```bash
mvn clean package
```

> Compiles your Spring Boot application and generates a JAR file in the `target/` directory.

---

## 🏗️ 2. Build the Docker Image

```bash
docker build -t thedevroom/spring-boot-mongo-application:1 .
```

> Packages your app into a Docker image with the specified tag.

---

## 🌐 3. Create a Custom Docker Network

```bash
docker network create appnetwork
```

> Creates a user-defined Docker bridge network to allow inter-container communication by name.

---

## 💾 4. Create a Persistent Volume for MongoDB

```bash
docker volume create mongodb
```

> Ensures MongoDB data is persisted across container restarts and removals.

---

## 🐘 5. Run MongoDB Container

```bash
docker run -d --name mongo --restart always --network appnetwork \
  -e MONGO_INITDB_ROOT_USERNAME=dbuser \
  -e MONGO_INITDB_ROOT_PASSWORD=dbadmin@123 \
  -v mongodb:/data/db \
  mongo
```

> Starts a MongoDB container with root credentials and connects it to the custom network with persistent storage.

---

## 🚀 6. Run Spring Boot Application Container

```bash
docker run -d --name springapp --restart always --network appnetwork \
  -e MONGO_DB_HOSTNAME=mongo \
  -e MONGO_DB_USERNAME=dbuser \
  -e MONGO_DB_PASSWORD=dbadmin@123 \
  -p 9090:8080 \
  thedevroom/spring-boot-mongo-application:1
```

> Launches the Spring Boot application container, configured to connect to MongoDB and exposed on port `9090`.

---

## 🌐 Application Access

Once both containers are running, open your browser and navigate to:

**👉 [http://localhost:9090/](http://localhost:9090/)**

---

## 🔎 Useful Docker Commands

### Check logs:
```bash
docker logs springapp
```

### Enter the container shell:
```bash
docker exec -it springapp /bin/sh
```

### View running containers:
```bash
docker ps
```

---

## 🧹 Cleanup Commands

To remove all containers, network, volumes, and the Docker image:

```bash
docker stop springapp mongo
docker rm springapp mongo
docker network rm appnetwork
docker volume rm mongodb
docker rmi -f thedevroom/spring-boot-mongo-application:1
```

> This stops and removes everything related to the app setup, leaving your system clean.

---

## 🔔 Notes & Best Practices

- ✅ **Start MongoDB first** so the application can connect without failure.
- 🔐 **Avoid hardcoding credentials** for production — use environment files or Docker secrets.
- ⚠️ **Ports & credentials** can be customized as needed.
- 🧪 **This setup is ideal for local testing**, not production deployment.

---

## ✅ Summary

This guide walks you through deploying a Spring Boot + MongoDB application using only Docker commands, without requiring Docker Compose or Kubernetes.

### Key Benefits:

- 🛠 No external tools — just Docker!
- 🔁 Easy to test and iterate locally.
- 🧱 Isolated networking for container security.
- 💾 Persistent MongoDB storage using Docker volumes.
- 📦 Clean and structured teardown process.

Perfect for developers who want a lightweight and reliable local environment without the overhead of orchestration tools.

---

#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---