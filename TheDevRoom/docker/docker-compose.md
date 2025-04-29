# 📦 Docker Compose – Complete Guide

Docker Compose is a powerful tool for defining and running multi-container Docker applications. This guide walks you through everything from basic setup to real-world examples.

---

## 📖 Table of Contents

1. [What is Docker Compose?](#-what-is-docker-compose)
2. [Why Use Docker Compose?](#-why-use-docker-compose)
3. [Prerequisites](#-prerequisites)
4. [Basic Concepts](#-basic-concepts)
5. [Sample Docker Compose File](#-sample-docker-compose-file)
6. [Running Docker Compose](#-running-docker-compose)
7. [Docker Compose Commands](#-docker-compose-commands)
8. [Real-World Example: Spring Boot + MongoDB](#-real-world-example-spring-boot--mongodb)
9. [Volumes, Networks, and Environment Variables](#-volumes-networks-and-environment-variables)
10. [Best Practices](#-best-practices)
11. [Resources](#-resources)

---

## ❓ What is Docker Compose?

**Docker Compose** is a tool for defining and running multi-container Docker applications using a **`docker-compose.yml`** file. You can configure services, networks, volumes, and more — all in one place.

---

## 💡 Why Use Docker Compose?

- Manage multiple containers easily
- Reproducible environments
- Simplifies local development
- Easily versioned with code
- Isolates app environments via networks and volumes

---

## ✅ Prerequisites

- Docker installed → [Install Docker](https://docs.docker.com/get-docker/)
- Docker Compose installed (`docker compose` or legacy `docker-compose`)
- Basic knowledge of Docker

---

## 🧱 Basic Concepts

| Concept       | Description                                                |
|---------------|------------------------------------------------------------|
| `services`    | Define containers (e.g., app, db)                          |
| `volumes`     | Persistent data storage                                    |
| `networks`    | Custom container communication networks                    |
| `build`       | Build image from Dockerfile                                |
| `image`       | Use a prebuilt Docker image                                |
| `depends_on`  | Define service start-up order                              |
| `environment` | Environment variables for containers                       |

---

## 📝 Sample Docker Compose File

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"

  redis:
    image: redis:alpine
```

---

## ▶️ Running Docker Compose

### Start services

```bash
docker compose up -d
```

### Stop services

```bash
docker compose down
```

---

## 🛠 Docker Compose Commands

```bash
# Start all services in background
docker compose up -d

# View logs for all services
docker compose logs -f

# Stop and remove all services
docker compose down

# Rebuild images (if using build context)
docker compose build

# Restart specific service
docker compose restart <service-name>

# Execute command inside container
docker compose exec <service-name> bash
```

---

## 🚀 Real-World Example: Spring Boot + MongoDB

### Directory Structure

```
springboot-mongodb-app/
├── Dockerfile
├── docker-compose.yml
├── src/
├── target/
```

### `docker-compose.yml`

```yaml
version: '3.8'

services:
  mongo:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: dbuser
      MONGO_INITDB_ROOT_PASSWORD: dbadmin@123
    networks:
      - spring-net

  app:
    image: thedevroom/spring-boot-mongo-application:1
    container_name: springapp
    ports:
      - "8080:8080"
    depends_on:
      - mongo
    environment:
      MONGO_DB_HOSTNAME: mongodb
      MONGO_DB_USERNAME: dbuser
      MONGO_DB_PASSWORD: dbadmin@123
    networks:
      - spring-net

networks:
  spring-net:
    driver: bridge
```

> Replace `thedevroom/spring-boot-mongo-application:1` with your image if needed.

---

## 📁 Volumes, Networks, and Environment Variables

### Add named volumes

```yaml
volumes:
  mongo-data:
```

Use in service:

```yaml
    volumes:
      - mongo-data:/data/db
```

---

## 🧠 Best Practices

- Use named volumes for persistent storage.
- Define custom networks for service isolation.
- Keep environment variables in `.env` file:

  ```
  MONGO_DB_USERNAME=dbuser
  MONGO_DB_PASSWORD=dbadmin@123
  ```

  Then in `docker-compose.yml`:

  ```yaml
  environment:
    - MONGO_DB_USERNAME=${MONGO_DB_USERNAME}
    - MONGO_DB_PASSWORD=${MONGO_DB_PASSWORD}
  ```

- Use `depends_on` to control startup order (note: does **not** wait for readiness).

---

## 📚 Resources

- 📘 [Docker Compose Docs](https://docs.docker.com/compose/)
- 📘 [Compose File Reference](https://docs.docker.com/compose/compose-file/)
- 📘 [Networking in Compose](https://docs.docker.com/compose/networking/)

---

With Docker Compose, managing complex app stacks becomes a breeze. Define it once, run it anywhere! 🐋

---

> 💡 Tip: Combine this with [Docker Networks](#) and [Docker Volume](#) guides for full-stack containerized workflows.

---
#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---
