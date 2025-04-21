# 🐳 Dockerfile Complete Guide

A **Dockerfile** is a script of instructions that automates the building of Docker images. This guide covers all key Dockerfile instructions, best practices, detailed examples, and steps to create and run Docker images.

---

## 📋 Prerequisites

Before working with Dockerfiles, ensure:

- Docker is installed (`docker -v`)
- Basic terminal knowledge
- Internet access (for pulling base images)
- Text editor like VS Code or Vim

---

## 🧱 Dockerfile Instructions Explained

### `FROM`
- **Purpose:** Specifies the base image.
- **Example:**
  ```dockerfile
  FROM ubuntu:20.04
  ```

### `MAINTAINER` *(Deprecated)*
- **Use `LABEL` instead.**
- **Example:**
  ```dockerfile
  LABEL maintainer="Your Name <you@example.com>"
  ```

### `LABEL`
- **Purpose:** Adds metadata.
- **Example:**
  ```dockerfile
  LABEL version="1.0" description="My Docker App"
  ```

### `COPY`
- **Purpose:** Copies local files/directories to image.
- **Example:**
  ```dockerfile
  COPY ./src /app/src
  ```

### `ADD`
- **Purpose:** Like COPY, but also handles remote URLs and auto-extracts archives.
- **Example:**
  ```dockerfile
  ADD https://example.com/app.tar.gz /opt/
  ```

### `RUN`
- **Purpose:** Executes commands during build.
- **Example:**
  ```dockerfile
  RUN apt-get update && apt-get install -y curl
  ```

### `CMD`
- **Purpose:** Default command when container starts.
- **Example:**
  ```dockerfile
  CMD ["npm", "start"]
  ```

### `ENTRYPOINT`
- **Purpose:** Configures container to run as executable.
- **Example:**
  ```dockerfile
  ENTRYPOINT ["java", "-jar", "app.jar"]
  ```

### `ENV`
- **Purpose:** Sets environment variables.
- **Example:**
  ```dockerfile
  ENV NODE_ENV=production
  ```

### `ARG`
- **Purpose:** Sets build-time variables.
- **Example:**
  ```dockerfile
  ARG app_env=prod
  ```

### `USER`
- **Purpose:** Defines which user to use.
- **Example:**
  ```dockerfile
  USER node
  ```

### `WORKDIR`
- **Purpose:** Sets working directory for instructions.
- **Example:**
  ```dockerfile
  WORKDIR /app
  ```

### `EXPOSE`
- **Purpose:** Informs Docker that the container listens on the specified port.
- **Example:**
  ```dockerfile
  EXPOSE 3000
  ```

### `VOLUME`
- **Purpose:** Mounts persistent volumes.
- **Example:**
  ```dockerfile
  VOLUME ["/data"]
  ```

---

## 🛠️ Best Practices

- Use minimal base images (e.g., `alpine`)
- Combine `RUN` commands to reduce layers
- Use `.dockerignore` to exclude unnecessary files
- Use `ENTRYPOINT` for executables and `CMD` for defaults
- Keep image sizes small

---

## 💡 Shell vs Exec Form

### Shell Form
```dockerfile
RUN apt-get update
CMD npm start
```
- Runs under `/bin/sh -c`
- Easier for string interpolation

### Exec Form
```dockerfile
CMD ["npm", "start"]
```
- Preferred for JSON array-style execution
- No shell processing

---

## 📁 Dockerfile Examples

### Node.js App
```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

### Multi-Stage Java Maven Build
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

## 🧪 Building and Running

### Build Image
```bash
docker build -t myapp:latest .
```

### Run Container
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

- Minimize layers to speed up builds
- Always pin image versions (`FROM ubuntu:20.04`)
- Use `.dockerignore` like `.gitignore`
- Add health checks in `docker-compose.yml` if applicable
- Use `COPY` over `ADD` unless extra features needed

---

## 📚 References

- [Dockerfile Docs](https://docs.docker.com/engine/reference/builder/)
- [Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker GitHub](https://github.com/docker/docker)

---

Happy Dockering! 🐳