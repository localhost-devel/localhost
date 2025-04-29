# 🐳 Docker Compose Setup for Spring Boot + MongoDB

Easily run your Spring Boot application alongside MongoDB using Docker Compose — a powerful tool for defining and running multi-container Docker applications.

---

## 🛠 Prerequisites

Ensure the following tools are installed on your system:

- 🐳 **Docker** – [Install Docker](https://docs.docker.com/get-docker/)
- ☕ **Java** and **Maven** – For building the Spring Boot project
- 📦 **docker-compose** – [Install Compose](https://docs.docker.com/compose/install/)
- 📦 **Spring Boot Application Repository** – Clone the application repository from GitHub:
    - [springboot-mongodb-app](https://github.com/localhost-devel/springboot-mongodb-app.git)

---

## 🔨 1. Build the Spring Boot Project

```bash
mvn clean package
```

> This command compiles your Spring Boot project and creates a `.jar` file inside the `target/` directory.

---

## 🛠 2. Build the Docker Image

```bash
docker build -t thedevroom/spring-boot-mongo-application:1 .
```

> Builds your Docker image using the `Dockerfile` and tags it as `thedevroom/spring-boot-mongo-application:1`.

---

## 🚀 3. Deploy the Application with Docker Compose

```bash
docker-compose up -d
```

> Starts both containers — Spring Boot and MongoDB — in detached mode.

---

## 📋 4. List Running Containers

```bash
docker ps -a
```

> Verifies that both the `springapp` and `mongodb` containers are up and running.

---

## 🌐 Application Access

Once deployed, access your application in the browser:

**Frontend URL** 👉 [http://localhost:9090/](http://localhost:9090/)

> Ensure your application listens on port `8080`, which is mapped to `9090` on your host.

---

## 🧹 Cleanup and Teardown

To stop and remove all Docker Compose services, volumes, and the custom image:

```bash
docker-compose down --volumes --remove-orphans
docker rmi -f thedevroom/spring-boot-mongo-application:1
```

> This command safely removes containers, volumes, and any orphaned resources.

---

## 📁 Sample `docker-compose.yml`

```yaml
version: '3.8'  # Use 3.8 or 3.9 if available

services:
  springapp:
    image: thedevroom/spring-boot-mongo-application:1
    restart: always
    environment:
      - MONGO_DB_HOSTNAME=mongo
      - MONGO_DB_USERNAME=dbuser
      - MONGO_DB_PASSWORD=dbadmin@123
    ports:
      - 9090:8080
    working_dir: /opt/app
    depends_on:
      - mongo
    networks:
      - appnetwork

  mongo:
    image: mongo
    restart: always
    environment:
      - MONGO_INITDB_ROOT_USERNAME=dbuser
      - MONGO_INITDB_ROOT_PASSWORD=dbadmin@123
    volumes:
      - mongodb:/data/db
    networks:
      - appnetwork

volumes:
  mongodb:
    external: false  # Create volume automatically

networks:
  appnetwork:
    external: false  # Create network automatically
```

> You can place this file in your project root as `docker-compose.yml`.

---

## 🔎 Additional Commands

### View logs from a service:
```bash
docker-compose logs -f springapp
```

### Rebuild image and restart services:
```bash
docker-compose up --build -d
```

### Enter the Spring Boot container:
```bash
docker exec -it $(docker ps -qf "name=springapp") /bin/sh
```

---

## ✅ Summary

This guide walks you through deploying a full Spring Boot + MongoDB stack using Docker Compose, ideal for local development and testing.

### Key Highlights:

- 🧱 **Multi-container orchestration** using `docker-compose`
- 🔄 **Simple lifecycle management** with `up`, `down`, `logs`, and `build`
- 🔐 **Environment variable configuration** for credentials and connectivity
- 💾 **MongoDB persistence** can be added using volumes if needed
- 📂 **Portable and reproducible** setup for dev teams

---

## 🔐 Best Practices (Production)

- Avoid hardcoded secrets — use `.env` files or Docker Secrets.
- Configure volumes for MongoDB to persist data between runs.
- Implement health checks and retry logic for production deployments.
- Use tagged image versions for better stability and control.

---

📦 With Docker Compose, you streamline both app and DB management, saving time and reducing complexity in your dev workflow.

#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---
