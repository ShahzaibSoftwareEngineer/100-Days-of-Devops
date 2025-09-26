# Day 44 of #100DaysOfDevOps — Write a Docker Compose File
## 📋 Lab Overview
Deploy a static website using Apache HTTP server with Docker Compose on App Server 3.

## 🎯 Lab Requirements
- **Server**: App Server 3 (stapp03.stratos.xfusioncorp.com)
- **User**: banner | **Password**: BigGr33n
- **Container Name**: httpd
- **Image**: httpd:latest
- **Port Mapping**: Host 8085 → Container 80
- **Volume Mapping**: `/opt/data` → `/usr/local/apache2/htdocs`
- **Compose File**: `/opt/docker/docker-compose.yml`

---

##  What is Docker Compose?
**Docker Compose** is a tool for defining and running multi-container applications using a YAML file instead of multiple `docker run` commands.

### Why Docker Compose?
- **Single Command**: `docker-compose up command ` vs multiple docker run commands
- **Reproducible**: Same environment everywhere
- **Easy Management**: Start, stop, scale services together

---

## 🚀 Step-by-Step Lab Implementation

### Step 1: Connect to Server
```bash
ssh banner@172.16.238.12
# Password: BigGr33n
```

### Step 2: Create Required Directories
```bash
sudo mkdir -p /opt/docker
cd /opt/docker
```

### Step 3: Create Docker Compose File
```bash
sudo nano docker-compose.yml
```

### Step 4: Add Configuration
```yaml
version: '3.8'

services:
  web:
    image: httpd:latest
    container_name: httpd
    ports:
      - "8085:80"
    volumes:
      - /opt/data:/usr/local/apache2/htdocs:ro
    restart: unless-stopped
```

### Step 5: Validate and Deploy
```bash
# Validate compose file
docker-compose config

# Start the container
docker-compose up -d

# Verify container is running
docker-compose ps
```

### Step 6: Test the Website
```bash
# Test locally
curl http://localhost:8085

# Check container logs
docker-compose logs

# View running processes
docker ps
```

---

## 🛠️ Essential Docker Compose Commands

### Basic Operations
```bash
# Start services in background
docker-compose up -d

# Stop all services
docker-compose down

# Restart services
docker-compose restart

# View service status
docker-compose ps

# View logs
docker-compose logs

# View real-time logs
docker-compose logs -f
```

---

## 🎯 Lab Success Checklist

- [ ] SSH connection to stapp03 successful
- [ ] `/opt/docker/docker-compose.yml` created
- [ ] Container named `httpd` running
- [ ] Port 8085 mapped to container port 80
- [ ] Volume `/opt/data` mounted to `/usr/local/apache2/htdocs`
- [ ] Website accessible via `http://localhost:8085`
- [ ] `docker-compose ps` shows container as "Up"

---

## 📚 Quick Reference

### File Structure
```
/opt/docker/
└── docker-compose.yml

/opt/data/
└── (static website files)
```

### Commands Used
```bash
docker-compose up -d      # Deploy
docker-compose down       # Stop
docker-compose ps         # Status
docker-compose logs       # View logs
curl http://localhost:8085  # Test
```
