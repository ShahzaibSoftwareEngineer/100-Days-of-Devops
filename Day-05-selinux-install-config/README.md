# 🚀 Day 5 of 100 Days of DevOps: SELinux Installation and Configuration

## 🎯 Challenge: 100 Days of DevOps with KodeKloud

## 🖥️ Server Details
| Server Name | IP Address | Hostname | User | Purpose |
|---

## 🔹 SELinux Overview

* **What is SELinux?**
  SELinux is a Linux kernel security module that enforces security policies using **labels** on files, processes, and ports.

* **Why SELinux?**
  Traditional Linux permissions (user, group, others) are not enough for advanced security. SELinux prevents processes from doing things they are not explicitly allowed to, even if the standard permissions allow it.

-------------|------------|----------|------|---------|
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Nautilus App 2 |

---

## 📋 Lab Overview
**Scenario:** xFusionCorp Industries security team has opted to enhance application and server security with SELinux following a security audit.

**Objective:** Install SELinux packages and permanently disable SELinux on App Server 2 for testing purposes.

---

## 🔧 Step-by-Step Solution

### Step 1: Connect to App Server 2
```bash
# SSH into App Server 2 (stapp02 - Nautilus App 2)
ssh steve@stapp02.stratos.xfusioncorp.com
# OR using IP address
ssh steve@172.16.238.11
```

### Step 2: Switch to Root User
```bash
# Switch to root for SELinux management
sudo su -
```

### Step 3: Check Current SELinux Status
```bash
# Check current SELinux status
sestatus

# Alternative command to check status
getenforce
```

### Step 4: Install SELinux Packages
```bash
# Install required SELinux packages
yum install -y policycoreutils policycoreutils-python selinux-policy selinux-policy-targeted libselinux-utils setroubleshoot-server setools setools-console mcstrans

# Alternative for newer systems
dnf install -y policycoreutils policycoreutils-python-utils selinux-policy selinux-policy-targeted libselinux-utils setroubleshoot-server setools setools-console mcstrans
```

### Step 5: Permanently Disable SELinux
```bash
# Edit SELinux configuration file
vi /etc/selinux/config

# Change SELINUX=enforcing to SELINUX=disabled
# The file should contain: SELINUX=disabled
```

### Step 6: Alternative Method to Disable SELinux
```bash
# Use sed command to change configuration
sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config

# Or use this command
sed -i 's/^SELINUX=permissive$/SELINUX=disabled/' /etc/selinux/config
```

### Step 7: Verify Configuration Changes
```bash
# Check the SELinux configuration file
cat /etc/selinux/config

# Verify the SELINUX parameter is set to disabled
grep "SELINUX=disabled" /etc/selinux/config
```

### Step 8: Check Current Runtime Status (Optional)
```bash
# Check current runtime status (will show current state)
sestatus

# Check enforcement mode
getenforce
```

---

## ✅ Validation Steps

1. ✅ SELinux packages installed successfully
2. ✅ SELinux configuration file modified (/etc/selinux/config)
3. ✅ SELINUX parameter set to 'disabled'
4. ✅ Configuration will take effect after scheduled reboot