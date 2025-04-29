# 🐳 Docker Networking – Complete Guide

A comprehensive, step-by-step guide to Docker networking including concepts, types, commands, examples, and real-world use cases.

---

## 📖 Table of Contents

1. [Understanding Application Architectures](#-understanding-application-architectures)
2. [Introduction to Containerization](#-introduction-to-containerization)
3. [What is Docker Networking?](#-what-is-docker-networking)
4. [Types of Docker Networks](#-types-of-docker-networks)
5. [Bridge Network (Default & Custom)](#-bridge-network-default--custom)
6. [Host Network](#-host-network)
7. [None Network](#-none-network)
8. [Overlay Network (Multi-Host)](#-overlay-network-multi-host)
9. [Useful Commands](#-useful-commands)
10. [Use Cases Comparison Table](#-docker-network-use-cases)
11. [Real-World Example](#-real-world-example)
12. [Best Practices](#-best-practices)
13. [Resources](#-resources)

---

## 📘 Understanding Application Architectures

### Monolithic Architecture
- A single application with all features bundled together.
- Built and deployed as **one unit**.
- **Challenges**:
    - Difficult to scale specific features.
    - Every change requires a full redeployment.
    - Harder to maintain and test over time.

### Microservices Architecture
- Application is broken into **multiple independent services**.
- Each service handles a specific business function.
- **Advantages**:
    - Independent deployments.
    - Easier to scale horizontally.
- **Challenges**:
    - Requires containerization for consistent environments.
    - Needs **CI/CD pipelines**, networking, and orchestration.

---

## 📦 Introduction to Containerization

Containerization tools allow packaging applications with their dependencies in isolated environments.

| Tool      | Description                            |
|-----------|----------------------------------------|
| Docker    | Industry-standard container platform   |
| Podman    | Daemonless container engine            |
| containerd| Runtime used by Docker and Kubernetes  |
| CRI-O     | Kubernetes-native runtime              |
| RKT       | CoreOS container tool (now deprecated) |

---

## 🌐 What is Docker Networking?

Docker Networking allows **containers to communicate**:
- With each other (container-to-container)
- With the host (container-to-host)
- With the outside world (internet access)

When a container is started, it's attached to a **network**. The network determines how it communicates and is isolated.

---

## 🕸️ Types of Docker Networks

Docker provides 5 main network drivers:

1. **bridge** – default for standalone containers on a single host.
2. **host** – uses the host's networking stack.
3. **none** – disables all networking.
4. **overlay** – connects containers across multiple Docker hosts.
5. **macvlan** – assigns a MAC address to a container (advanced use).

We'll focus on the most commonly used: `bridge`, `host`, `none`, and `overlay`.

---

## 🧱 Bridge Network (Default & Custom)

### 🔹 Default Bridge Network

- Containers on this network can communicate **only via IP addresses**.
- No automatic hostname resolution.

Example:

```bash
docker run -d --name container1 nginx
docker run -d --name container2 alpine sleep 1000
docker exec -it container2 ping 172.17.0.2   # Works
docker exec -it container2 ping container1   # Fails
```

### 🔸 Custom Bridge Network

- Allows **name-based communication** between containers.
- Each container can be referenced by its name.

Create a network:

```bash
docker network create -d bridge app-network
```

Run containers on this network:

```bash
docker run -d --name mongo --network app-network mongo
docker run -d --name springapp --network app-network springboot-app
```

Access between containers:

```bash
docker exec -it springapp ping mongo      # Hostname resolution works
```

---

## 🖥 Host Network

- The container shares the **host’s network stack**.
- No container-specific IP address – uses host IP.
- Direct access to host ports.

Example:

```bash
docker run -d --name nginx --network host nginx
```

> ⚠️ Only one container can bind to a port at a time on the host network.

> ✅ Suitable for performance-sensitive or host-aware applications.

---

## 🚫 None Network

- The container has **no network access**.
- Cannot connect to other containers, internet, or the host.

Example:

```bash
docker run -d --name isolated --network none alpine sleep 1000
```

> ✅ Useful for tasks requiring **maximum isolation**.

---

## 🌍 Overlay Network (Multi-Host)

- Used in **Docker Swarm mode** or with Kubernetes.
- Enables communication between containers **on different Docker hosts**.
- Built-in DNS-based service discovery.

Steps:

```bash
docker swarm init
docker network create -d overlay --attachable swarm-net
docker service create --name webapp --network swarm-net nginx
```

> ✅ Ideal for cloud-native and distributed applications.

---

## 🛠 Useful Commands

```bash
# List all networks
docker network ls

# Inspect a network
docker network inspect <network-name>

# Create a custom bridge network
docker network create -d bridge devnet

# Connect a container to a network
docker network connect devnet container1

# Disconnect a container from a network
docker network disconnect devnet container1

# Prune unused networks
docker network prune
```

---

## 📊 Docker Network Use Cases

| Network Type   | Hostname Resolution | Multi-Host Support | Use Case                                  |
|----------------|---------------------|--------------------|--------------------------------------------|
| Default Bridge | ❌ No                | ❌ No               | Basic, single-host containers              |
| Custom Bridge  | ✅ Yes               | ❌ No               | Microservices on a local Docker host       |
| Host           | N/A (host ports)     | ❌ No               | Performance-critical or host-integrated   |
| None           | ❌ No                | ❌ No               | Isolated tasks (security-sensitive)       |
| Overlay        | ✅ Yes               | ✅ Yes              | Multi-host Swarm/Kubernetes apps          |

---

## 🧪 Real-World Example

### Use Case: Spring Boot App with MongoDB

```bash
# Create a custom bridge network
docker network create -d bridge springapp-net

# Run MongoDB container
docker run -d --name mongodb --network springapp-net mongo

# Run Spring Boot app container
docker run -d --name springboot-app --network springapp-net -p 8080:8080 thedevroom/springboot-app

# Test connection from app to MongoDB
docker exec -it springboot-app ping mongodb
```

---

## ✅ Best Practices

- Always use **custom bridge networks** in development to enable hostname resolution.
- Prefer **overlay networks** in multi-host or Swarm environments.
- Avoid using the **host network** unless required for performance or hardware access.
- Use **`.env` files** or **Docker Secrets** for sensitive environment variables.
- Clean up unused networks periodically:

```bash
docker network prune
```

---

## 📚 Resources

- 📘 [Docker Networking Docs](https://docs.docker.com/network/)
- 📘 [Docker Compose Networking](https://docs.docker.com/compose/networking/)
- 📘 [Networking in Docker Swarm](https://docs.docker.com/engine/swarm/networking/)

---

💡 With proper networking, your Dockerized applications become scalable, flexible, and production-ready.

---

> Happy Networking! 🐋

---
#### 👨‍💻 Created by: TheDevRoom

- 🌐 Website: [TheDevRoom](https://github.com/localhost-devel/localhost-devel/blob/master/README.md)
- 📞 Contact: +91 9999999999
---