# Docker Lifecycle — Complete Flow

Think of Docker like this:

- Docker Engine = Factory manager
- Docker Image = Blueprint/template
- Docker Container = Running machine created from blueprint
- Docker Network = Roads/connectivity between machines
- Docker Volume = Storage room (persistent data)

---

# 1. Overall Docker Lifecycle

```text
Write Application
       ↓
Create Dockerfile
       ↓
Build Docker Image
       ↓
Store Image (Local/Docker Hub)
       ↓
Run Container from Image
       ↓
Attach Network/Volumes
       ↓
Start / Stop / Restart Container
       ↓
Scale Containers
       ↓
Delete Container
       ↓
Delete Image
```

---

# 2. Docker Image Lifecycle

## What is Image?

Image is a **read-only template**.

Example:
- Java installed
- Spring Boot JAR copied
- Port exposed
- Startup command defined

Image is similar to:
- Class in Java
- Blueprint of house
- ISO file of OS

---

## Image Lifecycle

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker push (optional)
    ↓
Docker Registry/DockerHub
    ↓
docker pull
    ↓
Create Containers
    ↓
Unused Image Cleanup
```

---

## Step-by-Step

---

## Step 1 — Create Dockerfile

Example:

```dockerfile
FROM eclipse-temurin:17-jdk

WORKDIR /app

COPY target/app.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
```

---

## Step 2 — Build Image

```bash
docker build -t myapp:v1 .
```

Now image gets created.

Check:

```bash
docker images
```

---

## Step 3 — Push Image

```bash
docker push myapp:v1
```

Stored in registry like:
- Docker Hub
- ECR
- GCR
- ACR

---

## Step 4 — Pull Image

Other machines:

```bash
docker pull myapp:v1
```

---

## Step 5 — Remove Image

```bash
docker rmi myapp:v1
```

---

# Internal Working of Image

Image is made of **layers**.

Example:

```text
Layer 1 → Ubuntu OS
Layer 2 → Java Installed
Layer 3 → Application JAR
Layer 4 → Startup Command
```

Benefits:
- Faster downloads
- Reuse layers
- Less storage

---

# 3. Docker Container Lifecycle

## What is Container?

Container = Running instance of Image.

Like:

```text
Java Class → Object
Docker Image → Container
```

---

# Container Lifecycle States

```text
Created
   ↓
Running
   ↓
Paused
   ↓
Stopped
   ↓
Removed
```

---

# Detailed Container Lifecycle

---

## 1. Create Container

```bash
docker create myapp:v1
```

Container created but NOT started.

Check:

```bash
docker ps -a
```

State:

```text
Created
```

---

## 2. Start Container

```bash
docker start container_id
```

State:

```text
Running
```

---

## 3. Run Container Directly

Most common:

```bash
docker run myapp:v1
```

Internally Docker does:

```text
docker create
docker start
```

---

## 4. Pause Container

```bash
docker pause container_id
```

Processes freeze.

---

## 5. Unpause

```bash
docker unpause container_id
```

---

## 6. Stop Container

Graceful shutdown:

```bash
docker stop container_id
```

Docker sends:

```text
SIGTERM
```

then after timeout:

```text
SIGKILL
```

---

## 7. Kill Container

Force stop:

```bash
docker kill container_id
```

Immediate termination.

---

## 8. Restart Container

```bash
docker restart container_id
```

---

## 9. Remove Container

```bash
docker rm container_id
```

---

# Important Container Commands

| Command | Purpose |
|---|---|
| docker ps | Running containers |
| docker ps -a | All containers |
| docker logs | View logs |
| docker exec | Enter container |
| docker inspect | Full details |
| docker stats | CPU/memory usage |

---

# Container Internal Working

When container starts:

Docker creates:
- Namespace
- Cgroups
- Writable layer
- Virtual network
- Filesystem mount

Then starts process:

```text
PID 1
```

Example:

```text
java -jar app.jar
```

If PID 1 exits:
→ Container stops.

---

# 4. Docker Network Lifecycle

# Why Network Needed?

Containers are isolated.

Network allows:
- Container ↔ Container communication
- Container ↔ Internet
- Host ↔ Container

---

# Network Lifecycle

```text
Create Network
      ↓
Attach Containers
      ↓
Communication
      ↓
Disconnect
      ↓
Remove Network
```

---

# Types of Docker Networks

| Type | Usage |
|---|---|
| bridge | Default local communication |
| host | Uses host network |
| none | No networking |
| overlay | Multi-host communication |
| macvlan | Real MAC/IP |

---

# Create Network

```bash
docker network create my-network
```

Check:

```bash
docker network ls
```

---

# Attach Container to Network

```bash
docker run --network my-network myapp:v1
```

---

# Connect Existing Container

```bash
docker network connect my-network container1
```

---

# Inspect Network

```bash
docker network inspect my-network
```

---

# Remove Network

```bash
docker network rm my-network
```

---

# Internal Working of Docker Network

Docker creates:
- Virtual Ethernet pairs (veth)
- Linux bridge
- IP allocation
- NAT rules
- DNS resolution

Example:

```text
Container A → 172.17.0.2
Container B → 172.17.0.3
```

Docker provides internal DNS:

```text
container-name
```

So containers can call:

```text
http://mysql-container:3306
```

---

# 5. Complete Real-Time Flow

Example:

Spring Boot + MySQL

---

# Step 1 — Build Image

```bash
docker build -t springboot-app .
```

---

# Step 2 — Create Network

```bash
docker network create app-network
```

---

# Step 3 — Start MySQL Container

```bash
docker run -d \
--name mysql \
--network app-network \
mysql:8
```

---

# Step 4 — Start Spring Boot Container

```bash
docker run -d \
-p 8080:8080 \
--name app \
--network app-network \
springboot-app
```

---

# Communication

```text
Browser
   ↓
Host Port 8080
   ↓
Spring Boot Container
   ↓
Docker Network
   ↓
MySQL Container
```

---

# 6. Relationship Between Image, Container, Network

```text
                Docker Image
                     ↓
            Create Container
                     ↓
         Attach Network/Volumes
                     ↓
             Running Process
```

---

# 7. Very Important Interview Concepts

---

## Image vs Container

| Image | Container |
|---|---|
| Blueprint | Running instance |
| Read-only | Read-write |
| Static | Dynamic |
| Reusable | Temporary |

---

## Why Container Stops?

Container lives until:
- Main process exits
- PID 1 exits

Example:

```bash
docker run ubuntu
```

Stops immediately because:
No foreground process running.

---

## Why Docker is Fast?

Because:
- Shares host OS kernel
- No full VM boot
- Lightweight namespaces

---

# 8. Docker Lifecycle in One Diagram

```text
               Dockerfile
                    ↓
             docker build
                    ↓
                 Image
                    ↓
               docker run
                    ↓
               Container
         ↙        ↓        ↘
    Network    Volumes    Logs
         ↓
   Container Communication
         ↓
      Stop/Remove
```

---

# 9. Most Important Commands Summary

## Images

```bash
docker build -t app .
docker images
docker pull nginx
docker push app
docker rmi app
```

---

## Containers

```bash
docker run app
docker ps
docker stop id
docker start id
docker restart id
docker rm id
docker logs id
docker exec -it id bash
```

---

## Networks

```bash
docker network ls
docker network create my-net
docker network inspect my-net
docker network rm my-net
```

---

# 10. Real Production Flow

In companies:

```text
Developer writes code
        ↓
CI/CD builds Docker image
        ↓
Push image to registry
        ↓
Kubernetes pulls image
        ↓
Creates containers (pods)
        ↓
Networks connect services
        ↓
Traffic routed via ingress
```

This is how modern microservices run in production.
