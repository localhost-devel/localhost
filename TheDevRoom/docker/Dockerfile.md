
# 🐳 Dockerfile – Complete Guide

A **Dockerfile** is a script of instructions that automates the building of Docker images. This guide covers all key Dockerfile instructions, best practices, detailed examples, and steps to create and run Docker images.

---

## 📖 Table of Contents

1. [Overview](#-overview)
2. [Prerequisites](#-prerequisites)
3. [Dockerfile Instructions](#-dockerfile-instructions)
  1. [FROM](#-from)
  2. [MAINTAINER (Deprecated)](#-maintainer-deprecated)
  3. [LABEL](#-label)
  4. [COPY](#-copy)
  5. [ADD](#-add)
  6. [RUN](#-run)
  7. [CMD](#-cmd)
  8. [ENTRYPOINT](#-entrypoint)
  9. [ENV](#-env)
  10. [ARG](#-arg)
  11. [USER](#-user)
  12. [WORKDIR](#-workdir)
  13. [EXPOSE](#-expose)
  14. [VOLUME](#-volume)
4. [Best Practices](#-best-practices)
5. [Shell vs Exec Form](#-shell-vs-exec-form)
6. [Dockerfile Examples](#-dockerfile-examples)
  1. [Node.js App Example](#-nodejs-app-example)
  2. [Multi-Stage Java Maven Build Example](#-multi-stage-java-maven-build-example)
7. [Building and Running Docker Images](#-building-and-running-docker-images)
8. [Dockerfile Considerations](#-dockerfile-considerations)
9. [Resources](#-resources)

---

## 📌 Overview

A **Dockerfile** is a text file containing instructions to automate the creation of Docker images. These instructions define the steps needed to set up an environment within the image, including dependencies, files, and commands. The Dockerfile is used with the `docker build` command to create Docker images that can later be run as containers.

---

## ✅ Prerequisites

Before working with Dockerfiles, ensure:

- Docker is installed (`docker -v`)
- Basic terminal knowledge
- Internet access (for pulling base images)
- Text editor like VS Code or Vim

---

## 🧱 Dockerfile Instructions

### 📝 `FROM`
- **Purpose:** Specifies the base image to use.
- **Example:**
  ```dockerfile
  FROM ubuntu:20.04
  ```

### 🧰 `MAINTAINER` *(Deprecated)*
- **Use `LABEL` instead.**
- **Example:**
  ```dockerfile
  LABEL maintainer="Your Name <you@example.com>"
  ```

### 🏷️ `LABEL`
- **Purpose:** Adds metadata to the image (e.g., version, description).
- **Example:**
  ```dockerfile
  LABEL version="1.0" description="My Docker App"
  ```

### 📂 `COPY`
- **Purpose:** Copies files/directories from the host system to the container.
- **Example:**
  ```dockerfile
  COPY ./src /app/src
  ```

### 📥 `ADD`
- **Purpose:** Similar to `COPY` but can handle remote URLs and auto-extracts archives.
- **Example:**
  ```dockerfile
  ADD https://example.com/app.tar.gz /opt/
  ```

### 🛠️ `RUN`
- **Purpose:** Executes commands during the image build process.
- **Example:**
  ```dockerfile
  RUN apt-get update && apt-get install -y curl
  ```

### ⌨️ `CMD`
- **Purpose:** Defines the default command to run when the container starts.
- **Example:**
  ```dockerfile
  CMD ["npm", "start"]
  ```

### 🎯 `ENTRYPOINT`
- **Purpose:** Configures the container to run as an executable.
- **Example:**
  ```dockerfile
  ENTRYPOINT ["java", "-jar", "app.jar"]
  ```

### 🌍 `ENV`
- **Purpose:** Sets environment variables inside the image.
- **Example:**
  ```dockerfile
  ENV NODE_ENV=production
  ```

### 🔧 `ARG`
- **Purpose:** Sets build-time variables that can be passed at build time.
- **Example:**
  ```dockerfile
  ARG app_env=prod
  ```

### 🧑‍🔧 `USER`
- **Purpose:** Specifies which user to run the container as.
- **Example:**
  ```dockerfile
  USER node
  ```

### 📂 `WORKDIR`
- **Purpose:** Sets the working directory for the following instructions.
- **Example:**
  ```dockerfile
  WORKDIR /app
  ```

### 🚪 `EXPOSE`
- **Purpose:** Informs Docker that the container listens on the specified port.
- **Example:**
  ```dockerfile
  EXPOSE 3000
  ```

### 💾 `VOLUME`
- **Purpose:** Mounts a persistent storage volume.
- **Example:**
  ```dockerfile
  VOLUME ["/data"]
  ```

---

## 🛠️ Best Practices

- **Minimize base image size:** Use smaller base images like `alpine`.
- **Consolidate `RUN` commands:** Minimize layers by combining commands.
- **Use `.dockerignore`:** Exclude unnecessary files to keep the image size smaller.
- **Use `ENTRYPOINT` for the executable and `CMD` for defaults.**
- **Keep images small:** Avoid installing unnecessary packages and files.

---

## 💡 Shell vs Exec Form

### 🗣️ Shell Form
- **Usage:** Allows string interpolation.
```dockerfile
RUN apt-get update
CMD npm start
```

### 🧑‍💻 Exec Form
- **Usage:** Preferred as it avoids shell processing.
```dockerfile
CMD ["npm", "start"]
```

---

## 📁 Dockerfile Examples

### Node.js App Example

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

### Multi-Stage Java Maven Build Example

```dockerfile
FROM maven:3.9 AS builder
WORKDIR /app
COPY . .
RUN mvn package

FROM openjdk:17-jdk
COPY --from=builder /app/target/app.jar /app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

---

## 🧪 Building and Running Docker Images

### Build the Image
```bash
docker build -t myapp:latest .
```

### Run the Container
```bash
docker run -d -p 8080:8080 myapp:latest
```

### Push to Docker Hub
```bash
docker tag myapp:latest username/myapp:latest
docker push username/myapp:latest
```

---

## 🧼 Dockerfile Considerations

- **Minimize layers:** Combine commands wherever possible to reduce layers.
- **Pin image versions:** Always use a specific image version (e.g., `FROM ubuntu:20.04`).
- **Use `.dockerignore`:** Similar to `.gitignore`, it helps avoid copying unnecessary files into the image.
- **Health checks:** Add health checks in `docker-compose.yml` for long-running containers.
- **Prefer `COPY` over `ADD`:** Use `COPY` unless `ADD`'s additional features are necessary.

---

## 📚 Resources

- [Dockerfile Docs](https://docs.docker.com/engine/reference/builder/)
- [Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker GitHub](https://github.com/docker/docker)

---

> Happy Dockering! 🐳

---
#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---