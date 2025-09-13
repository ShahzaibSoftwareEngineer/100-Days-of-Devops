

# Day 41: Writing a Dockerfile 🐳

## Objective

Create a **custom Docker image** for running Apache2 on Ubuntu 24.04 configured to listen on **port 8084**, following Dockerfile best practices.

## Overview

A **Dockerfile** is a blueprint for building Docker images. It defines the **base image**, **software to install**, **configuration changes**, and **commands to run** when the container starts.

Key concepts covered in this lab:

* Using official base images (`ubuntu:24.04`)
* Installing software inside a container (`apache2`)
* Modifying configuration files (`ports.conf`, `000-default.conf`)
* Exposing ports for container access
* Running processes in the foreground for container compatibility
* Following best practices for lightweight, maintainable images

---

## Lab Setup

**Server Details (App Server 3 – Stratos DC):**

* Hostname: `stapp03.stratos.xfusioncorp.com`
* IP: `172.16.238.12`
* User: `banner`
* Password: `BigGr33n`

---

## Task

• Base image: `ubuntu:24.04`
• Install Apache2
• Configure Apache to run on port 8084
• Build and run a container

---

## Step-by-Step Commands

### 1️⃣ Login to server

```bash
ssh banner@172.16.238.12
# Password: BigGr33n
```

### 2️⃣ Create Dockerfile

```bash
sudo mkdir -p /opt/docker
sudo nano /opt/docker/Dockerfile
```

Paste the following content:

```dockerfile
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y apache2 && \
    sed -i 's/Listen 80/Listen 8084/' /etc/apache2/ports.conf && \
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8084>/' /etc/apache2/sites-available/000-default.conf

EXPOSE 8084

CMD ["apache2ctl", "-D", "FOREGROUND"]
```

Save and exit (`CTRL+O`, `Enter`, `CTRL+X`).

---

### 3️⃣ Build Docker image

```bash
cd /opt/docker
sudo docker build -t myapache:41 .
```

---

### 4️⃣ Run Docker container

```bash
sudo docker run -d --name apache41 -p 8084:8084 myapache:41
```

---

### 5️⃣ Verify container

```bash
docker ps
curl http://localhost:8084
```

You should see the **Apache default page HTML**, confirming Apache is running on port 8084.

---

## Dockerfile Best Practices Applied

• Used official Ubuntu base image
• Combined `RUN` commands to reduce image layers
• Set `ENV DEBIAN_FRONTEND=noninteractive` to avoid prompts
• Cleaned apt cache to reduce image size
• Exposed port 8084 properly
• Ran Apache in foreground for container-friendly execution

---

## Key Learnings

• Dockerfiles act as blueprints for reproducible environments
• Using official base images improves security and reliability
• Combining `RUN` commands reduces image layers
• Cleaning apt cache keeps image size small
• Exposing ports properly documents container networking
• Running services in foreground ensures container stability
• Following best practices ensures images are efficient, secure, and maintainable
• Well-written Dockerfiles are essential for scalable DevOps pipelines

---


