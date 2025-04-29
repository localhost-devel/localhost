# 🐳 Docker Swarm – Complete Guide

Docker Swarm is Docker’s native orchestration tool for deploying, managing, and scaling containerized applications in a cluster of Docker nodes.

---

## 📚 Table of Contents

1. [📌 Overview](#-overview)
2. [⚙️ Architecture](#️-architecture)
3. [✅ Prerequisites](#-prerequisites)
4. [🚀 Setting Up Docker Swarm](#-setting-up-docker-swarm)
5. [🌐 Docker Swarm Networking](#-docker-swarm-networking)
6. [🧩 Docker Services: Global vs Replicated](#-docker-services-global-vs-replicated)
7. [📏 Scaling and Scheduling](#-scaling-and-scheduling)
8. [🏷️ Node Labels and Constraints](#-node-labels-and-constraints)
9. [🧰 Docker Stack Deployment](#-docker-stack-deployment)
10. [🔧 Monitoring and Maintenance](#-monitoring-and-maintenance)
11. [💡 Spring Boot + MongoDB Example](#-spring-boot--mongodb-example)
12. [🧾 Useful Commands](#-useful-commands)
13. [✅ Best Practices](#-best-practices)
14. [📚 Resources](#-resources)

---

## 📌 Overview

**Docker Swarm** is a container orchestration tool built into Docker Engine that allows you to manage multiple containers across multiple Docker hosts as a single virtual system.

---

## ⚙️ Architecture

- **Swarm Cluster**: Group of Docker engines (nodes) working together.
- **Manager Node**: Schedules tasks, maintains cluster state.
- **Worker Node**: Executes containers and reports to the manager.
- **Services**: Abstractions for containers. Can be **replicated** or **global**.
- **Tasks**: Actual containers running as part of a service.
- **Overlay Network**: Enables communication between nodes/services.

---

## ✅ Prerequisites

### 🖥️ System Requirements

- Minimum **3+ VMs or cloud instances** (e.g., AWS EC2, GCP VM, Azure VM)
- Docker installed on all nodes

```bash
docker --version
```

---

### 🔓 Open Required Ports

| Port        | Protocol | Purpose                        |
|-------------|----------|--------------------------------|
| 2377        | TCP      | Swarm cluster management       |
| 7946        | TCP/UDP  | Node discovery & communication |
| 4789        | UDP      | Overlay network traffic        |
| Protocol 50 | ESP      | Encrypted overlay networks *(optional)* |

---

### ⚙️ How to Open Ports

#### 🔥 UFW (Ubuntu Firewall)

```bash
sudo ufw allow 2377/tcp
sudo ufw allow 7946/tcp
sudo ufw allow 7946/udp
sudo ufw allow 4789/udp
sudo ufw allow proto esp
sudo ufw enable
```

#### 🌐 AWS EC2

1. Go to **EC2 > Security Groups**
2. Edit Inbound Rules:
  - TCP: 2377, 7946
  - UDP: 7946, 4789
  - Protocol: ESP (no port)
  - Source: Your VPC CIDR or 0.0.0.0/0 (for testing)

---

### ⚙️ Sample Setup Script

```bash
#!/bin/bash
sudo apt update -y
sudo apt install docker.io -y
sudo usermod -aG docker ubuntu
sudo systemctl start docker
```

---

## 🚀 Setting Up Docker Swarm

### Initialize Swarm (Manager Node)

```bash
docker swarm init --advertise-addr $(curl http://169.254.169.254/latest/meta-data/public-ipv4)
```

### Get Worker Join Token

```bash
docker swarm join-token worker
```

### Join Workers

```bash
docker swarm join --token <token> <manager-ip>:2377
```

### Check Cluster

```bash
docker node ls
```

---

## 🌐 Docker Swarm Networking

### Create Overlay Network

```bash
docker network create -d overlay springoverlay
```

---

## 🧩 Docker Services: Global vs Replicated

### Global Services

```bash
docker service create --name global-agent --mode global -p 9090:9090 thedevroom/node-monitor:1
```

### Replicated Services

```bash
docker service create --name replicated-app --replicas 3 -p 8080:8080 thedevroom/java-app:1
```

---

## 📏 Scaling and Scheduling

### Scale Services

```bash
docker service scale replicated-app=5
```

### View Tasks

```bash
docker service ps replicated-app
```

---

## 🏷️ Node Labels and Constraints

### Add Label

```bash
docker node update --label-add type=workerNode <NodeID>
```

### Deploy with Constraint

```bash
docker service create --name webapp --replicas 2 \
--constraint "node.labels.type==workerNode" \
-p 8080:8080 thedevroom/web-app:1
```

---

## 🧰 Docker Stack Deployment

### Example `docker-compose.yml`

```yaml
version: '3.8'

services:
  springboot:
    image: thedevroom/spring-boot-mongo:latest
    environment:
      - MONGO_DB_HOSTNAME=mongo
      - MONGO_DB_USERNAME=devdb
      - MONGO_DB_PASSWORD=devdb123
    ports:
      - 8080:8080
    depends_on:
      - mongo
    networks:
      - springappnetwork
    deploy:
      replicas: 2
      placement:
        constraints:
          - node.role==worker
      update_config:
        parallelism: 1
        delay: 20s
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 5

  mongo:
    image: mongo
    environment:
      - MONGO_INITDB_ROOT_USERNAME=devdb
      - MONGO_INITDB_ROOT_PASSWORD=devdb123
    volumes:
      - mongodb:/data/db
    networks:
      - springappnetwork

volumes:
  mongodb:
    external: true

networks:
  springappnetwork:
    external: true
```

### Deploy Stack

```bash
docker network create -d overlay springappnetwork
docker stack deploy --compose-file docker-compose.yml springapp
```

---

## 🔧 Monitoring and Maintenance

### Drain Node

```bash
docker node update <NodeID> --availability drain
```

### Activate Node

```bash
docker node update <NodeID> --availability active
```

### Promote/Demote Nodes

```bash
docker node promote <NodeID>
docker node demote <NodeID>
```

---

## 💡 Spring Boot + MongoDB Example

```bash
docker stack deploy -c docker-compose.yml springapp
docker stack services springapp
docker stack ps springapp
```

---

## 🧾 Useful Commands

```bash
# Cluster inspection
docker node ls
docker node inspect <NodeID>

# Services and tasks
docker service ls
docker service ps <service>
docker stack ls
docker stack services <stack>
docker stack ps <stack>

# Cleanup
docker service rm <service>
docker stack rm <stack>

# Watch real-time
watch docker stack ps springapp
```

---

## ✅ Best Practices

- Always use labels for node-level constraints
- Deploy with `docker stack` using Compose files
- Use separate overlay networks for stack isolation
- Define `restart_policy` and `update_config` for resilience
- Prefer external volume drivers in production
- Monitor cluster health and node availability regularly

---

## 📚 Resources

- [Docker Swarm Official Docs](https://docs.docker.com/engine/swarm/)
- [Docker Stack Reference](https://docs.docker.com/engine/reference/commandline/stack_deploy/)
- [Docker Compose File Reference](https://docs.docker.com/compose/compose-file/compose-versioning/)
- [Docker Swarm Networking](https://docs.docker.com/network/overlay/)


---
#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---
