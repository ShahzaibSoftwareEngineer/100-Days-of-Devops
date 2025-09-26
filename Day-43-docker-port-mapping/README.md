# Day 43: Docker Port Mapping - Nginx Container Lab 🐳


### Understanding Docker Networking

Docker containers run in isolated networks with their own private IPs. This keeps them secure but means you can’t access them directly from outside.

### What is Port Mapping?

Port mapping connects a port on the host to a port inside the container, letting you access container apps from your machine or network.


---

## 📋 Lab Task
Deploy an nginx web server is  a Docker container with proper port mapping configuration on Application Server 1.

## 🎯 Lab Requirements
- Pull the `nginx:stable` Docker image from Docker Hub
- Create a new container named `blog` using the nginx image
- Configure port mapping: host port `8085` → container port `80`
- Ensure the container remains in running state
- Verify external accessibility of the nginx service

---

## 💻 Lab Commands

### Step 1: Pull the nginx stable image
```bash
docker pull nginx:stable
```

### Step 2: Create container with port mapping
```bash
docker run -d --name blog -p 8085:80 nginx:stable
```

### Step 3: Verify container status
```bash
docker ps
```

### Step 4: Test the web server
```bash
curl http://localhost:8085
```

### Step 5: Check port mapping details
```bash
docker port blog
```

---

## 📖 Command Breakdown

**`docker pull nginx:stable`**
- Downloads the nginx image with 'stable' tag from Docker Hub
- Ensures we have a specific, stable version rather than latest

**`docker run -d --name blog -p 8085:80 nginx:stable`**
- `docker run` → Creates and starts a new container
- `-d` → Detached mode (runs in background)
- `--name blog` → Assigns  a friendly name "blog" to the container
- `-p 8085:80` → Port mapping (host:container format)
- `nginx:stable` → Specifies the image to use

**`docker ps`**
- Lists all currently running containers
- Shows container ID, image, ports, and status

**`curl http://localhost:8085`**
- Tests HTTP connectivity to the mapped port
- Should return nginx welcome page HTML

**`docker port blog`**
- Displays port mapping information for the 'blog' container
- Confirms which host ports are connected to container ports

---

## ✅ Expected Lab Results

### Successful Container Creation
```bash
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  PORTS                  NAMES
abc123def456   nginx:stable   "/docker-entrypoint.…"   0.0.0.0:8085->80/tcp   blog
```

### Port Mapping Verification
```bash
$ docker port blog
80/tcp -> 0.0.0.0:8085
```

### Web Server Response
```bash
$ curl http://localhost:8085
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...

