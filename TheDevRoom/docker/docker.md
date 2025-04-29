
# 🐳 Docker Installation and Usage Guide

Docker is a powerful open-source platform for building, running, and managing containerized applications. This guide walks you through the complete setup, configuration, and usage of Docker with practical examples and best practices.

---

## 📋 Prerequisites

Before getting started, ensure you have:

- A Linux-based system (e.g., Ubuntu/Debian/CentOS)
- Sudo or root access to install packages
- Basic understanding of shell commands
- Internet access for installing packages

---

## 🚀 Installation Guide

### 🔧 Step 1: Install Docker Engine on Ubuntu

```bash
curl -fsSL get.docker.com | /bin/bash
```
- Downloads and runs the official Docker installation script (quick method).

OR

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
# Press CTRL+C to exit status view
```
- Installs Docker and its dependencies using APT package manager.
- Starts and enables Docker service to run at boot.

### 👥 Add User to Docker Group

```bash
sudo usermod -aG docker $USER
```
- Adds your user to the Docker group to allow running `docker` without `sudo`.

**Important**: After running the above command, log out and back in OR run:
```bash
su - $USER
```
to apply group changes.

---

## 🔍 Basic Docker Commands

### 🔧 Docker Version and Info

```bash
docker version
```
- Displays Docker client and server version.

```bash
docker info
```
- Shows detailed information about the Docker setup.

### 🔐 Login to Docker Registry

```bash
docker login
```
- Authenticates your local Docker client to Docker Hub or other registries.

---

## 📦 Working with Docker Images

### 📜 List Docker Images

```bash
docker images
```
- Lists all locally available Docker images.

```bash
docker images -f dangling=true
```
- Lists dangling images (untagged, unused images).

### ❌ Remove Images

```bash
docker rmi <image_id>
```
- Removes a specific image by ID or name.

```bash
docker rmi $(docker images -q)
```
- Removes all images returned by `docker images -q` (all image IDs).

```bash
docker rmi -f $(docker images -q)
```
- Force removes all images, even if used by stopped containers.

### 🏷️ Tag Images

```bash
docker tag <image_id> myrepo/myapp:v1.0
```
- Tags an existing image with a new name and version.

### ⬆️⬇️ Push and Pull Images

```bash
docker push myrepo/myapp:v1.0
docker pull myrepo/myapp:v1.0
```
- Push uploads an image to the registry. Pull downloads it.

### 🧱 Image Layers

```bash
docker history myapp:latest
```
- Shows each layer used to build the image.

### 🧹 Remove Dangling Images

```bash
docker image prune
```
- Deletes all dangling images to save space.

---

## 📦 Docker Containers

### 🆕 Create Container

```bash
docker create --name mycontainer myapp:latest
```
- Creates a container from an image without starting it.

### ▶️ Run a Container

```bash
docker run --name mycontainer myapp:latest
```
- Creates and starts a container.

### 🌐 Run with Port Mapping

```bash
docker run -d -p 8080:80 --name webserver nginx
```
- Runs container in background with port 8080 mapped to 80.

### 🔄 Start/Restart/Stop/Remove Containers

```bash
docker start <container_name>
docker restart <container_name>
docker stop <container_name>
docker rm <container_name>
```
- Start/restart/stop or remove containers.

### 📜 List Containers

```bash
docker ps
docker ps -a
```
- Lists running (`ps`) or all (`-a`) containers.

### 🛑 Kill Container

```bash
docker kill <container_id>
```
- Forces the container to stop immediately.

### 🧹 Remove All Containers

```bash
docker rm $(docker ps -aq)
docker rm -f $(docker ps -aq)
```
- Removes all containers. Add `-f` to force remove running ones.

### 🔁 Rename, Pause, and Unpause Containers

```bash
docker rename old_name new_name
docker pause <container_id>
docker unpause <container_id>
```
- Rename or suspend/resume container execution.

### 🧽 Prune Stopped Containers

```bash
docker container prune
```
- Deletes all stopped containers.

---

## 🛠️ Container Troubleshooting and Management

### 🔍 Inspect Container Details

```bash
docker inspect <container_name>
```
- Shows low-level details of the container in JSON format.

### 📄 View Logs

```bash
docker logs <container_name>
docker logs -f <container_name>
docker logs <container_name> --tail 10
docker logs <container_name> >> logs.txt
```
- View and follow logs or write to file.

### 🔎 View Processes in Container

```bash
docker top <container_name>
```
- Lists processes running inside the container.

### 📊 Container Resource Stats

```bash
docker stats
```
- Shows real-time resource usage of running containers.

### 🧠 Limit CPU & Memory

```bash
docker run --memory="512m" --cpus="1.0" myapp
```
- Restricts container to 512MB RAM and 1 CPU.

### 🧰 Execute Command in Running Container

```bash
docker exec -it <container_name> /bin/bash
```
- Run an interactive shell in a running container.

### 📌 Save Container State as Image

```bash
docker commit <container_name> myapp:snapshot
```
- Saves current container state as a new image.

### 📂 Copy Files Between Host and Container

```bash
docker cp ./config.txt mycontainer:/app/config.txt
docker cp mycontainer:/app/logs.txt ./logs.txt
```
- Copies files to/from container.

### 🔗 Attach to Container

```bash
docker attach <container_name>
```
- Attach to the running container's main process.

---

## 🧹 Docker Cleanup

### 🧹 Remove All Unused Resources

```bash
docker system prune
```
- Cleans up all unused containers, networks, images, and cache.

### 🧹 Remove Unused Volumes

```bash
docker volume prune
```
- Removes all unused volumes.

---

## 🧠 Best Practices
- Keep images small and use official base images.
- Use tags and versioning for image management.
- Run one process per container.
- Clean up unused images and containers regularly.

---

## 🐙 Docker Registries

- **Docker Hub**: https://hub.docker.com/
- **GitHub Container Registry**: https://github.com/features/packages
- **Self-hosted (e.g., Harbor, Nexus, etc.)**

---

## 📚 Helpful Resources

- [Docker Official Documentation](https://docs.docker.com/)
- [Docker GitHub Repo](https://github.com/docker/docker-ce)
- [Play with Docker (Labs)](https://labs.play-with-docker.com/)
- [Awesome Docker Resources](https://github.com/veggiemonk/awesome-docker)

---

Happy Containerizing! 🚀


#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---
