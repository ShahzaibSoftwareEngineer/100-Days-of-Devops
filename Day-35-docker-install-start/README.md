
# Day 35 of 100 Days of DevOps — Install Docker Packages & Start Docker Service

## 📌 Topic
Docker installation and service setup for App Server 1 as part of containerizing applications.

---

## 📝 What I Learned
- How to install **Docker CE** and **Docker Compose** on Linux servers.
- How to start and enable the Docker service using `systemctl`.
- Preparing servers for containerized application deployment and testing.

---

## ⚡ Step-by-Step Commands

### 1. Connect to App Server 1

```bash

ssh tony@172.16.238.10
# password: Ir0nM@n

````

### 2. Update packages

```bash

sudo yum update -y

```

### 3. Install Docker prerequisites

```bash

sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

```

### 4. Install Docker CE and Docker Compose

```bash

sudo yum install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

```

### 5. Start and enable Docker service

```bash

sudo systemctl start docker
sudo systemctl enable docker

```

### 6. Verify installation

```bash

sudo systemctl status docker
docker --version
docker compose version

```

---

## 🖥 Server Details

| Server  | IP Address    | Hostname                        | User | Password | Role           |
| ------- | ------------- | ------------------------------- | ---- | -------- | -------------- |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Ir0nM\@n | Nautilus App 1 |

---

## 🔗 Resources

* [Docker Official Documentation](https://docs.docker.com/get-docker/)
* [Docker Compose Documentation](https://docs.docker.com/compose/)

---



