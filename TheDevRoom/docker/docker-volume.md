# 📦 Docker Volumes Guide

> Complete guide to Docker Volumes for data persistence, sharing, and backup in containerized environments.

---

## 📚 Table of Contents

1. [🧐 Why Use Docker Volumes?](#-why-use-docker-volumes)
2. [🧱 Types of Volumes](#-types-of-volumes)
   - [🔗 Bind Mounts](#a-bind-mounts)
   - [📁 Docker Named Volumes](#b-docker-named-volumes)
   - [🌐 External (Network) Volumes](#-external-network-volumes)
3. [💾 Volume Use Cases](#-volume-use-cases)
4. [🧪 Setup Examples](#-setup-examples)
5. [🔒 Read-Only Volumes](#-read-only-volumes)
6. [🧠 Best Practices](#-best-practices)
7. [🧰 Useful Commands](#-useful-commands)
8. [📚 Resources](#-resources)

---

## 🧐 Why Use Docker Volumes?

Volumes are essential for persistent, shareable storage in Docker environments:

- 📦 Store and retain data beyond container lifecycle
- 🔄 Share data between containers and host
- ♻️ Backup, restore, or migrate data easily
- 🚀 Decouple data from application code

---

## 🧱 Types of Volumes

### 🔗 a) Bind Mounts

Mount specific host directory into a container:

```bash
mkdir /home/ubuntu/MongoDB

docker run -d --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb@123 \
  -v /home/ubuntu/MongoDB:/data/db \
  --network springappnetwork \
  mongo
```

✅ Verify:

```bash
docker exec mongo ls /data/db
```

---

### 📁 b) Docker Named Volumes

Managed by Docker in `/var/lib/docker/volumes`:

```bash
docker volume create -d local mongodbvol

docker run -d --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb@123 \
  -v mongodbvol:/data/db \
  --network springappnetwork \
  mongo
```

🔍 Inspect:

```bash
docker volume inspect mongodbvol
sudo ls /var/lib/docker/volumes/mongodbvol/_data
```

---

### 🌐 External (Network) Volumes

Cloud volumes or plugins like `rexray`, useful for production/high availability.

#### Example: AWS EBS via REX-Ray

```bash
docker plugin install rexray/ebs \
  EBS_ACCESSKEY=<access_key> \
  EBS_SECRETKEY=<secret_key>

docker volume create -d rexray/ebs mongodbebsvol

docker run -d --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb@123 \
  -v mongodbebsvol:/data/db \
  --network springappnetwork \
  mongo
```

---

## 💾 Volume Use Cases

- 🧠 **MongoDB/PostgreSQL**: Store databases at `/data/db` or `/var/lib/postgresql`
- 📑 **Configs & Logs**: Share logs with host for monitoring
- 🔄 **Backups**: Take snapshots of volumes for quick restores
- 🤝 **Multi-container apps**: Share files across containers (e.g. NGINX + PHP)

---

## 🧪 Setup Examples

```bash
# Clone Spring Boot Mongo app
git clone https://github.com/example/spring-boot-mongo-docker
cd spring-boot-mongo-docker
mvn clean package
docker build -t myapp/spring-boot-mongo:1 .

# Create network
docker network create -d bridge springappnetwork

# Create volume and run MongoDB
docker volume create mongodbvol
docker run -d --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb@123 \
  -v mongodbvol:/data/db \
  --network springappnetwork \
  mongo

# Run Spring Boot App
docker run -d --name springapp \
  -p 8080:8080 \
  -e MONGO_DB_HOSTNAME=mongo \
  -e MONGO_DB_USERNAME=devdb \
  -e MONGO_DB_PASSWORD=devdb@123 \
  --network springappnetwork \
  myapp/spring-boot-mongo:1
```

---

## 🔒 Read-Only Volumes

Mount volumes as read-only using `:ro` flag:

```bash
docker run -d --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=devdb \
  -e MONGO_INITDB_ROOT_PASSWORD=devdb@123 \
  -v mongodbebsvol:/data/db:ro \
  --network springappnetwork \
  mongo
```

---

## 🧠 Best Practices

- ✅ Use **named volumes** for automated, portable storage
- 🚫 Avoid writing app data inside the container filesystem
- 📦 Backup volumes using `tar`, `rsync`, or volume plugins
- 🔄 Schedule periodic snapshots for critical data
- ☁️ Use **external drivers/plugins** in production
- 📊 Monitor usage with `docker system df -v`

---

## 🧰 Useful Commands

```bash
docker volume ls                          # List all volumes
docker volume create <name>              # Create volume
docker volume inspect <name>             # View details
docker volume rm <name>                  # Delete a volume
docker volume prune                      # Clean up unused volumes
docker inspect <container>              # View volume mounts
```

---

## 📚 Resources

- 📘 [Docker Volumes Documentation](https://docs.docker.com/storage/volumes/)
- 🔌 [Docker Plugin System](https://docs.docker.com/engine/extend/plugins/)
- ☁️ [RexRay Plugin Guide](https://rexray.readthedocs.io/)
- 🧪 [Spring Boot + Mongo Docker Example](https://github.com/example/spring-boot-mongo-docker)

---

🚀 Happy Volumizing!

---
#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---