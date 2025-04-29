# 🐳 Docker Installation & Usage Guide

> Comprehensive Docker guide for beginners to advanced users. Includes setup, basic and advanced usage, cleanup, and best practices.

---

## 📚 Table of Contents

1. [📋 Prerequisites](#-prerequisites)
2. [🚀 Installation Guide](#-installation-guide)
    - [🔧 Install Docker Engine](#-step-1-install-docker-engine-on-ubuntu)
    - [👥 Add User to Docker Group](#-add-user-to-docker-group)
3. [🔍 Basic Docker Commands](#-basic-docker-commands)
4. [📦 Working with Docker Images](#-working-with-docker-images)
5. [📦 Docker Containers](#-docker-containers)
6. [🛠️ Container Troubleshooting and Management](#️-container-troubleshooting-and-management)
7. [🧹 Docker Cleanup](#-docker-cleanup)
8. [🧠 Best Practices](#-best-practices)
9. [🐙 Docker Registries](#-docker-registries)
10. [📚 Helpful Resources](#-helpful-resources)

---

## 📋 Prerequisites

Make sure the following requirements are met before proceeding:

- 🐧 Linux OS (Ubuntu/Debian/CentOS preferred)
- 🔐 Sudo/root access
- 🌐 Internet connection
- 💡 Basic shell knowledge

---

## 🚀 Installation Guide

### 🔧 Step 1: Install Docker Engine on Ubuntu

#### 📦 Quick Script Installation (Recommended)

```bash
curl -fsSL https://get.docker.com | bash
```

#### 🛠️ Manual APT Installation

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```

---

### 👥 Add User to Docker Group

```bash
sudo usermod -aG docker $USER
su - $USER
```

> 💡 Log out and log back in or run `su - $USER` to apply changes.

---

## 🔍 Basic Docker Commands

```bash
docker version        # Display Docker version
docker info           # Docker setup details
docker login          # Login to Docker registry
```

---

## 📦 Working with Docker Images

```bash
docker images                         # List images
docker rmi <image_id>                # Remove image
docker tag <image_id> repo/app:1.0   # Tag image
docker push repo/app:1.0             # Push image to registry
docker pull repo/app:1.0             # Pull image
docker image prune                   # Remove dangling images
docker history app:latest            # Show image layers
```

---

## 📦 Docker Containers

```bash
docker create --name myapp myimage:1.0          # Create container
docker run --name myapp myimage:1.0             # Run container
docker run -d -p 8080:80 nginx                  # Detached run with port mapping
docker start/stop/restart <name>               # Control containers
docker rm <name>                               # Remove container
docker ps / docker ps -a                       # List containers
docker rename old new                          # Rename container
docker pause/unpause <container>               # Pause/unpause
docker kill <container>                        # Force stop
docker exec -it <container> /bin/bash          # Access shell inside container
docker cp ./file container:/path               # Copy to container
docker cp container:/path ./file               # Copy from container
docker commit mycontainer app:snapshot         # Save state as image
docker attach <container>                      # Attach to terminal
docker logs <container> --tail 10              # Tail logs
docker top <container>                         # Running processes
docker stats                                   # Resource usage
docker container prune                         # Remove stopped containers
```

---

## 🛠️ Container Troubleshooting and Management

```bash
docker inspect <container_name>         # Inspect container config
docker logs <container>                 # View container logs
docker logs -f <container>              # Follow logs
docker logs <container> >> logs.txt     # Save logs to file
```

---

## 🧹 Docker Cleanup

```bash
docker system prune             # Remove all unused data
docker image prune              # Remove unused images
docker container prune          # Remove stopped containers
docker volume prune             # Remove unused volumes
```

---

## 🧠 Best Practices

- Use **lightweight base images** (e.g., `alpine`, `debian-slim`)
- Always **tag your images** (avoid `latest` in production)
- Use **multi-stage builds** to reduce image size
- Keep containers **single-purpose**
- Regularly clean up unused images, containers, and volumes
- Use `.dockerignore` to exclude files from builds
- Use **volumes** for persistent storage
- Pin base image versions for consistency

---

## 🐙 Docker Registries

| Registry        | Description                        |
|----------------|------------------------------------|
| Docker Hub      | Official Docker public registry    |
| GitHub Packages | Container registry via GitHub      |
| Harbor/Nexus    | Enterprise-grade private registries |

```bash
docker login
docker tag app myregistry/app:v1
docker push myregistry/app:v1
docker pull myregistry/app:v1
```

---

## 📚 Helpful Resources

- 📘 [Docker Docs](https://docs.docker.com/)
- 💻 [Play with Docker](https://labs.play-with-docker.com/)
- 🚀 [Docker GitHub](https://github.com/docker/docker-ce)
- 🌱 [Awesome Docker](https://github.com/veggiemonk/awesome-docker)


---
> Happy Containerizing! 🚀

---
#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---