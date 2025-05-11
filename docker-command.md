# 🚀 Microservices Docker Setup

This repository contains Docker commands and instructions for setting up a microservices-based application. The services include:

- **Service Registry (Eureka)**
- **Admin Server**
- **Welcome Service**
- **Greet Service**
- **API Gateway**

---

## 🧠 Prerequisites

Before starting, ensure you have the following:

- Docker installed and running.
- A custom Docker network for service discovery.

To create the Docker network, run:

```bash
docker network create eureka-net
```

---

## 📦 Microservices Setup

### 1️⃣ Service Registry (Eureka)

Run the Eureka Service Registry:

```bash
docker run -d \
  --name service-registry \
  --network eureka-net \
  -p 8761:8761 \
  service-registry
```

---

### 2️⃣ Admin Server

Run the Admin Server:

```bash
docker run -d \
  --name admin-server \
  --network eureka-net \
  -p 1111:1111 \
  admin-server \
  java -jar app.jar --server.port=1111
```

---

### 3️⃣ Welcome Service

Build and run the Welcome Service:

```bash
# Build the Docker image
docker build -t welcome-service .

# Run the container
docker run -d \
  --name welcome-service \
  --network eureka-net \
  -p 8080:8080 \
  welcome-service
```

---

### 4️⃣ Greet Service

Build and run the Greet Service:

```bash
# Build the Docker image
docker build -t greet-service .

# Run the container
docker run -d \
  --name greet-service-9080 \
  --network eureka-net \
  -p 9080:9080 \
  greet-service \
  java -jar app.jar --server.port=9080
```

---

### 5️⃣ API Gateway

Build and run the API Gateway:

```bash
# Build the Docker image
docker build -t api-gateway-service .

# Run the container
docker run -d \
  --name api-gateway-service-3333 \
  --network eureka-net \
  -p 3333:3333 \
  api-gateway-service \
  java -jar app.jar --server.port=3333
```

---

## ✅ Final Steps

All services are now running in the `eureka-net` network and can communicate using Eureka discovery.

To ensure proper functionality:

- Verify the `application.yml` or `application.properties` for each service is correctly configured for:
  - Eureka Service Registry
  - Admin Server
  - Zipkin (if applicable)

---

## 📝 Notes

- For troubleshooting, check the logs of each service using:

  ```bash
  docker logs <container-name>
  ```

- Make sure all necessary Docker images are built and available in your environment.

Happy coding! ✨
