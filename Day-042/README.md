# Day 42: Docker Network Creation Lab - Complete Guide

## 🎯 Lab Overview
**Objective**: Create a custom Docker network on App Server 3 in the Nautilus DevOps environment with specific network configurations.

---

## 📋 Task Requirements

### Primary Objectives:
- Create a Docker network named `media` on App Server 3
- Configure network to use bridge driver
- Set subnet to `10.10.1.0/24`
- Set IP range to `10.10.1.0/24`

### Server Information:
| Server | IP Address | Hostname | User | Purpose |
|--------|------------|----------|------|---------|
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | Nautilus App Server 3 |

---

## 🔍 Docker Networking 

### What is Docker Networking?
Docker networking enables communication between containers, between containers and the host, and between containers and external networks. It provides isolation, security, and flexibility for containerized applications.

### Key Networking Concepts:

#### 1. **Network Drivers**
- **Bridge**: Default driver for standalone containers on single host
- **Host**: Removes network isolation between container and host
- **Overlay**: Multi-host networking for Swarm services
- **Macvlan**: Assigns MAC addresses to containers
- **None**: Disables networking completely

#### 2. **Bridge Networks**
- Creates a software-defined network on the host
- Containers on same bridge can communicate
- Provides automatic DNS resolution between containers
- Isolates containers from external networks by default

#### 3. **Subnet and IP Range**
- **Subnet**: Defines the network address space (e.g., 10.10.1.0/24)
- **IP Range**: Specifies which IPs Docker can assign to containers
- **CIDR Notation**: /24 means 24 bits for network, 8 bits for hosts (256 addresses)

#### 4. **Network Isolation Benefits**
- **Security**: Separate network segments prevent unauthorized access
- **Resource Management**: Better control over network traffic
- **Service Discovery**: Containers can find each other by name
- **Load Balancing**: Distribute traffic across multiple instances

---

## 🛠️ Implementation Steps

### Step 1: Environment Connection
```bash
# SSH connection to App Server 3
ssh banner@stapp03.stratos.xfusioncorp.com
# Password: BigGr33n
```

### Step 2: System Preparation
```bash
# Switch to root user for Docker operations
sudo su -

# Verify Docker service status
systemctl status docker

# Start Docker if not running
systemctl start docker
systemctl enable docker
```

### Step 3: Pre-Implementation Analysis
```bash
# Check current Docker networks
docker network ls

# Expected output shows default networks:
# NETWORK ID     NAME      DRIVER    SCOPE
# <id>           bridge    bridge    local
# <id>           host      host      local
# <id>           none      null      local
```

### Step 4: Network Creation
```bash
# Create the media network with specified configurations
docker network create --driver bridge --subnet=10.10.1.0/24 --ip-range=10.10.1.0/24 media
```

**Command Breakdown:**
- `docker network create`: Creates a new network
- `--driver bridge`: Specifies bridge driver for single-host networking
- `--subnet=10.10.1.0/24`: Defines network address space
- `--ip-range=10.10.1.0/24`: Sets available IP range for containers
- `media`: Network name

### Step 5: Verification and Validation
```bash
# Verify network creation
docker network ls

# Detailed network inspection
docker network inspect media

# Check specific network parameters
docker network inspect media | grep -E "Subnet|IPRange|Driver"
```

---

## 📊 Expected Results

### Network Creation Output:
```bash
$ docker network create --driver bridge --subnet=10.10.1.0/24 --ip-range=10.10.1.0/24 media
7f8a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1v2w3x4y5z6a7b8c9d0e
```

### Network List Output:
```bash
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
abc123def456   bridge    bridge    local
789ghi012jkl   host      host      local
media123456    media     bridge    local
345mno678pqr   none      null      local
```

### Network Inspection Output:
```json
[
    {
        "Name": "media",
        "Id": "media123456...",
        "Created": "2024-XX-XXTXX:XX:XX.XXXXXXX",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "10.10.1.0/24",
                    "IPRange": "10.10.1.0/24"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

---

## 🧪 Testing and Validation

### Test Container Creation:
```bash
# Create test container using the media network
docker run -dit --name test-container --network media alpine:latest

# Verify container network assignment
docker inspect test-container | grep -A 10 "NetworkSettings"

# Check container's IP address
docker exec test-container ip addr show
```

### Network Connectivity Test:
```bash
# Create second test container
docker run -dit --name test-container-2 --network media alpine:latest

# Test communication between containers
docker exec test-container ping test-container-2
```

---

## 📈 Advanced Concepts

### Network Security Considerations:
- Bridge networks provide isolation between different networks
- Containers on same network can communicate freely
- External access requires port mapping (`-p` flag)
- Consider using custom networks instead of default bridge

### Production Best Practices:
- Use descriptive network names
- Document subnet allocations
- Implement network segmentation strategy
- Monitor network performance and usage
- Regular cleanup of unused networks

### Integration with Orchestration:
```yaml
# Docker Compose example
version: '3.8'
services:
  web:
    image: nginx
    networks:
      - media
  db:
    image: mysql
    networks:
      - media

networks:
  media:
    external: true
```

---

## 🧹 Cleanup Commands

### Remove Test Containers:
```bash
# Stop and remove test containers
docker stop test-container test-container-2
docker rm test-container test-container-2
```

### Remove Network (if needed):
```bash
# Remove the media network
docker network rm media

# Force removal if containers are attached
docker network rm media --force
```

---

*This lab is part of the #100DaysOfDevOps challenge, focusing on practical Docker networking skills essential for modern containerized applications and microservices architecture.*