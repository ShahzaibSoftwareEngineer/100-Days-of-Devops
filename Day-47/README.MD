# 🐳 Day 47: Docker Python App - Complete Lab Solution

## 📋 Task Overview
Successfully dockerized and deployed a Python application on App Server 1 with proper containerization, port mapping, and deployment configuration.

### 🎯 Objectives Completed
- ✅ Created optimized Dockerfile with Python base image
- ✅ Built custom Docker image `nautilus/python-app`
- ✅ Deployed container `pythonapp_nautilus` with port mapping
- ✅ Configured application to run on container port 8082 → host port 8098
- ✅ Verified deployment with successful curl testing

---

## 🛠️ Implementation Steps

### Step 1: Project Structure Setup
```
/python_app/
├── Dockerfile           # ← Created this
└── src/
    ├── requirements.txt # ← Pre-existing
    ├── server.py       # ← Pre-existing
    └── [app files]     # ← Pre-existing
```

### Step 2: Dockerfile Configuration
```dockerfile
# Use Python 3.9 slim image as base
FROM python:3.9-slim

# Set working directory inside the container
WORKDIR /app

# Copy requirements file first (for better caching)
COPY src/requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the entire src directory to the container
COPY src/ .

# Expose port 8082
EXPOSE 8082

# Run the server.py script
CMD ["python", "server.py"]
```

### Step 3: Build and Deploy Commands
```bash
# Navigate to project directory
cd /python_app

# Build the Docker image
docker build -t nautilus/python-app .

# Create and run the container
docker run -d \
  --name pythonapp_nautilus \
  -p 8098:8082 \
  nautilus/python-app

# Test the deployment
curl http://localhost:8098/
```

---

## 🔍 Verification Results

### ✅ Image Creation
```bash
$ docker images | grep nautilus/python-app
nautilus/python-app   latest   abc123def456   2 minutes ago   150MB
```

### ✅ Container Status
```bash
$ docker ps
CONTAINER ID   IMAGE                 COMMAND           STATUS        PORTS                    NAMES
xyz789abc123   nautilus/python-app   "python server.py"   Up 1 minute   0.0.0.0:8098->8082/tcp   pythonapp_nautilus
```

### ✅ Application Response
```bash
$ curl http://localhost:8098/
{"status": "success", "message": "Python app is running!"}
```

---

## 🎓 Key Learning Points

### 🔧 Docker Best Practices Applied
- **Multi-stage optimization**: Copied requirements.txt first for better layer caching
- **Slim base image**: Used `python:3.9-slim` to reduce image size
- **No-cache pip**: Used `--no-cache-dir` to keep image lean
- **Proper working directory**: Set `/app` as WORKDIR for organization

### 🌐 Networking Configuration
- **Container port**: 8082 (internal application port)
- **Host port**: 8098 (external access port)
- **Port mapping**: `-p 8098:8082` for proper traffic routing

### 📦 Container Management
- **Named container**: `pythonapp_nautilus` for easy identification
- **Detached mode**: `-d` flag for background execution
- **Process management**: CMD instruction for proper application startup

---

## 🛡️ Troubleshooting Guide

### Common Issues & Solutions

**Issue**: Container exits immediately
```bash
# Solution: Check logs
docker logs pythonapp_nautilus
```

**Issue**: Port already in use
```bash
# Solution: Check port usage
sudo netstat -tulpn | grep 8098
# Use different port: -p 8099:8082
```

**Issue**: Dependencies not installing
```bash
# Solution: Verify requirements.txt
cat /python_app/src/requirements.txt
# Rebuild with verbose output
docker build -t nautilus/python-app . --progress=plain
```

---

