# 🚀 Day 68: Set Up Jenkins Server

## 📌 Overview
Successfully completed the Jenkins server installation and configuration for CI/CD pipeline implementation at xFusionCorp Industries. This marks a crucial step in automating our build, test, and deployment processes.

---

## 🎯 What is Jenkins?

**Jenkins** is an open-source automation server that enables developers to build, test, and deploy their applications reliably and efficiently. It's the leading tool in Continuous Integration/Continuous Deployment (CI/CD) practices.

### Key Features:
- **Automation**: Automates repetitive tasks in software development
- **Integration**: Supports 1800+ plugins for integration with various tools
- **Distributed Builds**: Can distribute work across multiple machines
- **Pipeline as Code**: Define build pipelines using Groovy-based DSL
- **Extensible**: Highly customizable through plugins and configurations

### Why Jenkins?
- ✅ Free and open-source
- ✅ Large community support
- ✅ Platform-independent (runs on Java)
- ✅ Easy to install and configure
- ✅ Extensive plugin ecosystem

---

## 🛠️ Lab Objectives

1. Install Jenkins on CentOS/RHEL using `yum` utility
2. Configure and start Jenkins service
3. Complete initial setup wizard
4. Create admin user with specific credentials

---

## 📋 Implementation Steps

### 1️⃣ Initial Server Access
```bash
# Connect to Jenkins server from jump host
ssh root@jenkins
# Password: S3curePass
```

### 2️⃣ Install Java (Jenkins Prerequisite)
```bash
# Jenkins 2.528.1 requires Java 17
sudo yum install fontconfig java-17-openjdk -y

# Verify installation
java -version
```

### 3️⃣ Add Jenkins Repository
```bash
# Download Jenkins repository configuration
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo

# Import Jenkins GPG key
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

### 4️⃣ Install Jenkins
```bash
# Install Jenkins using yum
sudo yum install jenkins -y
```

### 5️⃣ Configure Service (Troubleshooting)
```bash
# Create necessary directories
sudo mkdir -p /var/log/jenkins
sudo chown -R jenkins:jenkins /var/log/jenkins

# Set JAVA_HOME and increase timeout
sudo mkdir -p /etc/systemd/system/jenkins.service.d/
sudo cat > /etc/systemd/system/jenkins.service.d/override.conf << EOF
[Service]
Environment="JAVA_HOME=/usr/lib/jvm/jre-17-openjdk"
TimeoutStartSec=300
EOF

# Reload systemd configuration
sudo systemctl daemon-reload
```

### 6️⃣ Start Jenkins Service
```bash
# Enable Jenkins to start on boot
sudo systemctl enable jenkins

# Start Jenkins service
sudo systemctl start jenkins

# Verify service status
sudo systemctl status jenkins
```

### 7️⃣ Get Initial Admin Password
```bash
# Retrieve the initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### 8️⃣ Complete Web Setup
1. Access Jenkins UI at `http://<server-ip>:8080`
2. Unlock Jenkins with initial admin password
3. Install suggested plugins
4. Create admin user:
   - **Username**: `theadmin`
   - **Password**: `Adm!n321`
   - **Full Name**: `Siva`
   - **Email**: `siva@jenkins.stratos.xfusioncorp.com`
5. Configure Jenkins URL
6. Start using Jenkins

---

## 🔧 Technical Configuration

### Service Details
```yaml
Service Name: jenkins.service
Port: 8080
User: jenkins (UID: 994)
Home Directory: /var/lib/jenkins
Log Directory: /var/log/jenkins
Java Version: OpenJDK 17
Jenkins Version: 2.528.1
```

### System Requirements Met
- ✅ Java 17 installed
- ✅ Port 8080 available
- ✅ Sufficient memory (1GB+ allocated)
- ✅ Proper file permissions
- ✅ Service enabled for auto-start

---


## 📊 Results

### Service Status
```
● jenkins.service - Jenkins Continuous Integration Server
   Loaded: loaded (enabled)
   Active: active (running)
   Port:   8080 (listening)
   Memory: 1.0GB
   Status: All plugins prepared and loaded
```

### Admin User Created
```
Username:  theadmin
Full Name: Siva
Email:     siva@jenkins.stratos.xfusioncorp.com
Status:    Active
```

---

## 🎓 Key Learnings

1. **Dependency Management**: Jenkins requires Java runtime environment
2. **Service Configuration**: Understanding systemd service management and overrides
3. **Permission Management**: Proper file ownership crucial for service operation
4. **Troubleshooting**: Reading logs and error messages to identify issues
5. **Security**: Initial password mechanism and admin user creation

---
