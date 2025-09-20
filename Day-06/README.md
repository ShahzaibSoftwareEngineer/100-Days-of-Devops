# 🚀 Day 6 of 100 Days of DevOps: Create a Cron Job

## 🎯 Challenge: 100 Days of DevOps with KodeKloud

## 🖥️ Server Details
| Server Name | IP Address | Hostname | User | Purpose |
|-------------|------------|----------|------|---------|
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | Nautilus App 3 |

---

## 📋 Lab Overview
**Scenario:** Nautilus system admins team has prepared scripts to automate day-to-day tasks and wants to deploy them on a set schedule.

**Objective:** Install cronie package on all app servers, start crond service, and add a sample cron job for root user.

---

## 🔹 Cron Jobs Overview

* **What is Cron?**
  Cron is a time-based job scheduler in Linux that allows you to run commands or scripts automatically at specified times and intervals.

* **Why Use Cron?**
  Automates repetitive tasks like backups, system monitoring, log rotation, and maintenance without manual intervention. Essential for system administration and DevOps automation.

---

## 🔧 Step-by-Step Solution

### App Server 1 (stapp01)

#### Step 1: Connect to App Server 1
```bash
# SSH into App Server 1
ssh tony@stapp01.stratos.xfusioncorp.com
```

#### Step 2: Switch to Root User
```bash
# Switch to root for cron management
sudo su -
```

#### Step 3: Install Cronie Package
```bash
# Install cronie package
yum install -y cronie

# Alternative for newer systems
dnf install -y cronie
```

#### Step 4: Start and Enable Crond Service
```bash
# Start crond service
systemctl start crond

# Enable crond service to start on boot
systemctl enable crond

# Check service status
systemctl status crond
```

#### Step 5: Add Cron Job for Root User
```bash
# Open root's crontab
crontab -e

# Add the following line:
# */5 * * * * echo hello > /tmp/cron_text
```

#### Step 6: Verify Cron Job
```bash
# List current cron jobs for root
crontab -l

# Check if crond is running
ps aux | grep crond
```

### App Server 2 (stapp02)

#### Step 1: Connect to App Server 2
```bash
# SSH into App Server 2
ssh steve@stapp02.stratos.xfusioncorp.com
```

#### Step 2: Switch to Root User
```bash
# Switch to root for cron management
sudo su -
```

#### Step 3: Install Cronie Package
```bash
# Install cronie package
yum install -y cronie
```

#### Step 4: Start and Enable Crond Service
```bash
# Start crond service
systemctl start crond

# Enable crond service to start on boot
systemctl enable crond

# Check service status
systemctl status crond
```

#### Step 5: Add Cron Job for Root User
```bash
# Open root's crontab
crontab -e

# Add the following line:
# */5 * * * * echo hello > /tmp/cron_text
```

#### Step 6: Verify Cron Job
```bash
# List current cron jobs for root
crontab -l
```

### App Server 3 (stapp03)

#### Step 1: Connect to App Server 3
```bash
# SSH into App Server 3
ssh banner@stapp03.stratos.xfusioncorp.com
```

#### Step 2: Switch to Root User
```bash
# Switch to root for cron management
sudo su -
```

#### Step 3: Install Cronie Package
```bash
# Install cronie package
yum install -y cronie
```

#### Step 4: Start and Enable Crond Service
```bash
# Start crond service
systemctl start crond

# Enable crond service to start on boot
systemctl enable crond

# Check service status
systemctl status crond
```

#### Step 5: Add Cron Job for Root User
```bash
# Open root's crontab
crontab -e

# Add the following line:
# */5 * * * * echo hello > /tmp/cron_text
```

#### Step 6: Verify Cron Job
```bash
# List current cron jobs for root
crontab -l
```

### Final Verification (All Servers)
```bash
# Wait for 5 minutes and check if file is created
ls -la /tmp/cron_text

# Check file contents
cat /tmp/cron_text

# Monitor cron logs
tail -f /var/log/cron
```

---

## ✅ Validation Steps

1. ✅ Cronie package installed on all three app servers
2. ✅ Crond service started and enabled on all servers
3. ✅ Cron job added for root user (runs every 5 minutes)
4. ✅ Cron job creates /tmp/cron_text file with "hello" content