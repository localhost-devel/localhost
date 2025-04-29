
# 📦 Docker Volumes Guide

Docker Volumes are a key part of Docker’s data persistence and sharing strategy. This guide provides a complete overview of Docker volumes, their types, use cases, setup, and best practices.

---

## 📚 Table of Contents

1. [Why Use Docker Volumes?](#why-use-docker-volumes)
2. [Types of Volumes](#types-of-volumes)
    - [Local Volumes](#1-local-volumes)
    - [External (Network) Volumes](#2-external-network-volumes)
3. [Volume Use Cases](#volume-use-cases)
4. [Setup Examples](#setup-examples)
5. [Read-Only Volumes](#read-only-volumes)
6. [Best Practices](#best-practices)
7. [Useful Commands](#useful-commands)
8. [Resources](#resources)

---

## 🧐 Why Use Docker Volumes?

Volumes allow containers to persist and share data across restarts and between containers.

- Decouples application logic from storage.
- Useful for sharing config/logs between containers and host.
- Data is preserved even if containers are removed.
- Better for backups and recovery than container storage.

---

## 🧱 Types of Volumes

### 1) Local Volumes

#### a) Bind Mounts
Bind mounts map a specific file/folder on the host machine to a path inside the container.

```bash
mkdir /home/ubuntu/MongoDB

docker run -d --name mongo   -e MONGO_INITDB_ROOT_USERNAME=devdb   -e MONGO_INITDB_ROOT_PASSWORD=devdb@123   -v /home/ubuntu/MongoDB:/data/db   --network springappnetwork   mongo
```

Verify:

```bash
docker exec mongo ls /data/db
```

#### b) Docker Named Volumes (Persistent Volumes)

Docker manages the volume storage at `/var/lib/docker/volumes`.

```bash
docker volume create -d local mongodbvol

docker run -d --name mongo   -e MONGO_INITDB_ROOT_USERNAME=devdb   -e MONGO_INITDB_ROOT_PASSWORD=devdb@123   -v mongodbvol:/data/db   --network springappnetwork   mongo
```

Inspect volume:

```bash
docker volume inspect mongodbvol
sudo ls /var/lib/docker/volumes/mongodbvol/_data
```

---

### 2) External (Network) Volumes

Volumes stored outside the host. Used for production and high-availability scenarios.

#### Example: rex-ray plugin with AWS EBS

```bash
docker plugin install rexray/ebs   EBS_ACCESSKEY=<access_key>   EBS_SECRETKEY=<secret_key>

docker volume create -d rexray/ebs mongodbebsvol

docker run -d --name mongo   -e MONGO_INITDB_ROOT_USERNAME=devdb   -e MONGO_INITDB_ROOT_PASSWORD=devdb@123   -v mongodbebsvol:/data/db   --network springappnetwork   mongo
```

---

## 💾 Volume Use Cases

- **MongoDB container data**: Mount `/data/db` using volume for persistence.
- **App logs/configs**: Share log files with the host.
- **Backup/restore**: Easy snapshot/restore using volume drivers.
- **Multi-container apps**: Share config between frontend and backend containers.

---

## 🧪 Setup Examples

```bash
# Build Spring Boot + Mongo app
git clone https://github.com/example/spring-boot-mongo-docker
cd spring-boot-mongo-docker
mvn clean package
docker build -t myapp/spring-boot-mongo:1 .

# Create network
docker network create -d bridge springappnetwork

# Start Mongo container with volume
docker volume create mongodbvol
docker run -d --name mongo   -e MONGO_INITDB_ROOT_USERNAME=devdb   -e MONGO_INITDB_ROOT_PASSWORD=devdb@123   -v mongodbvol:/data/db   --network springappnetwork   mongo

# Start Spring app container
docker run -d --name springapp   -p 8080:8080   -e MONGO_DB_HOSTNAME=mongo   -e MONGO_DB_USERNAME=devdb   -e MONGO_DB_PASSWORD=devdb@123   --network springappnetwork   myapp/spring-boot-mongo:1
```

---

## 🔒 Read-Only Volumes

```bash
docker run -d --name mongo   -e MONGO_INITDB_ROOT_USERNAME=devdb   -e MONGO_INITDB_ROOT_PASSWORD=devdb@123   -v mongodbebsvol:/data/db:ro   --network springappnetwork   mongo
```

Use `:ro` at the end of the volume path to mount it as read-only.

---

## 🧠 Best Practices

- Use named volumes for portability and automation.
- Avoid storing data inside containers.
- Backup volumes using tools like `rsync`, `tar`, or volume plugins.
- For production, prefer external volumes or cloud-native plugins.
- Monitor volume usage with `docker system df -v`.

---

## 🧰 Useful Commands

```bash
docker volume ls                     # List all volumes
docker volume inspect <vol_name>    # Inspect volume metadata
docker volume prune                 # Remove all unused volumes
docker volume rm <vol_name>         # Remove a specific volume
docker inspect <container>          # See volume bindings
```

---

## 📚 Resources

- [Docker Volume Documentation](https://docs.docker.com/storage/volumes/)
- [Docker Plugin System](https://docs.docker.com/engine/extend/plugins/)
- [Rex-Ray Volume Plugin](https://rexray.readthedocs.io/)
- [Spring Boot + MongoDB Docker Example](https://github.com/example/spring-boot-mongo-docker)

---

Happy Volumizing! 🚀

#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---