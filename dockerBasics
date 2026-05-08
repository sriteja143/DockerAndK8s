# Docker Deep Learning Journey

> Learn Docker from installation to internals — pin to pin understanding.

---

# Why Learn Docker?

Docker is one of the most important tools for modern software engineering.

It helps developers:
- Package applications
- Run consistently across environments
- Avoid dependency issues
- Deploy easily
- Scale applications

---

# What Problem Docker Solves

Before Docker:

Applications worked differently on:
- Developer machine
- QA environment
- Production server

Classic issue:

```text
"It works on my machine."
```

Reason:
- Different OS
- Different Java version
- Different libraries
- Different configurations

Docker solved this by packaging:
- Application
- Dependencies
- Runtime
- Configurations

into a single portable unit.

---

# Virtual Machines Before Docker

Before containers, companies used Virtual Machines (VMs).

Architecture:

```text
Laptop/Server
   |
Hypervisor
   |
--------------------------------
| VM1 | VM2 | VM3 |
--------------------------------

Each VM contains:
- Full OS
- Kernel
- Libraries
- Application
```

## Problems with VMs

- Heavy
- Slow startup
- High memory usage
- Storage wastage

Example:
- Each VM may consume several GBs.

---

# Containers After Docker

Docker introduced lightweight containers.

Architecture:

```text
Host OS
   |
Docker Engine
   |
--------------------------------
| Container1 |
| Container2 |
| Container3 |
--------------------------------
```

Containers share:
- Same host OS kernel

This makes them:
- Lightweight
- Fast
- Efficient

---

# Docker is NOT a Virtual Machine

Very important concept:

```text
Container != VM
```

Container is just:
- Isolated process
- Using host kernel

---

# Linux Concepts Behind Docker

Docker is built using Linux kernel features.

| Linux Feature | Purpose |
|---|---|
| Namespaces | Isolation |
| cgroups | Resource limits |
| Union Filesystem | Layered images |
| chroot | Filesystem isolation |

Without Linux:
Docker would not exist in its current form.

---

# Namespaces

Namespaces provide isolation.

Example:
A container thinks:

```text
"I am the only process running."
```

Even though many containers exist.

## Types of Namespaces

| Namespace | Purpose |
|---|---|
| PID | Process isolation |
| NET | Network isolation |
| MNT | Filesystem isolation |
| IPC | Inter-process communication |
| UTS | Hostname isolation |
| USER | User isolation |

---

# Example of PID Namespace

Inside container:

```bash
ps -ef
```

You may see only few processes.

But host system may contain thousands.

This is process isolation.

---

# cgroups (Control Groups)

cgroups limit resources.

Examples:
- CPU limit
- Memory limit
- Disk IO limit

Without cgroups:
One container could consume entire server resources.

Example:

```bash
docker run -m 512m nginx
```

Limits container memory to:
- 512 MB

---

# Docker Architecture

```text
Docker Client
      |
REST API
      |
Docker Daemon (dockerd)
      |
--------------------------------
| Images |
| Containers |
| Networks |
| Volumes |
--------------------------------
```

---

# Docker Components

# 1. Docker Client

CLI commands:

```bash
docker ps
docker run
docker images
```

Client communicates with daemon.

---

# 2. Docker Daemon

Background service:

```text
dockerd
```

Responsible for:
- Containers
- Images
- Networks
- Volumes

---

# 3. Docker Registry

Stores Docker images.

Example:
- Docker Hub

Like:
- GitHub for Docker images

---

# 4. Docker Image

Read-only template.

Example:
- nginx image
- mysql image

Contains:
- Libraries
- Runtime
- Dependencies
- Application

---

# 5. Docker Container

Running instance of image.

Very important:

```text
Image = Blueprint
Container = Running Process
```

---

# What Happens Internally in docker run

Command:

```bash
docker run nginx
```

Internal steps:

1. Docker client sends request to daemon
2. Daemon checks local image
3. If absent → pulls from Docker Hub
4. Creates writable container layer
5. Creates namespaces
6. Applies cgroups
7. Starts process

---

# Installing Docker on Mac

Download Docker Desktop from:

https://www.docker.com/products/docker-desktop/

Install Docker Desktop.

---

# Verify Installation

Check version:

```bash
docker --version
```

Check daemon:

```bash
docker info
```

---

# Run First Container

```bash
docker run hello-world
```

This verifies:
- Docker client
- Docker daemon
- Registry connectivity
- Container runtime

---

# Important Docker Commands

# Check Images

```bash
docker images
```

---

# Check Running Containers

```bash
docker ps
```

---

# Check All Containers

```bash
docker ps -a
```

---

# Remove Container

```bash
docker rm <containerId>
```

---

# Remove Image

```bash
docker rmi <imageId>
```

---

# Docker Internal Storage

Docker stores:
- Images
- Layers
- Metadata
- Volumes
- Networks

Linux path:

```text
/var/lib/docker
```

On Mac:
Stored inside Docker VM.

---

# Docker Image Layers

Docker images are layered.

Example:

```text
Ubuntu Layer
   +
Java Layer
   +
Application Layer
```

Benefits:
- Reusability
- Faster builds
- Storage optimization

---

# Example of Layer Sharing

Two Java applications:
- Share same Java layer
- Save storage space

---

# Docker Learning Roadmap

| Phase | Topic |
|---|---|
| 1 | Docker basics |
| 2 | Images |
| 3 | Containers |
| 4 | Docker internals |
| 5 | Networking |
| 6 | Volumes |
| 7 | Dockerfile |
| 8 | Docker Compose |
| 9 | Security |
| 10 | Optimization |
| 11 | Spring Boot Dockerization |
| 12 | Kubernetes relation |

---

# Homework

Install Docker Desktop.

Run these commands:

```bash
docker --version

docker info

docker run hello-world

docker images

docker ps -a
```

Observe:
- Image download
- Container creation
- Exited container state

---

# Next Learning Topic

Container Lifecycle Deep Dive:
- create
- run
- start
- stop
- kill
- restart
- pause
- remove

Including:
- Writable layers
- PID 1 issue
- Filesystem isolation
- Process isolation
- Kernel interaction
