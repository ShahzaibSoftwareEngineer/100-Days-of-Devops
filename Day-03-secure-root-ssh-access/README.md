# 🚀 Day 3 of 100 Days of DevOps: Secure Root SSH Access

## 🎯 Challenge: 100 Days of DevOps with KodeKloud

## 🖥️ Server Details
| Server Name | IP Address | Hostname | User | Purpose |
|-------------|------------|----------|------|---------|
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | Nautilus App 3 |

---

## 📋 Lab Overview
**Scenario:** xFusionCorp Industries security team has rolled out new protocols following security audits, including restriction of direct root SSH login.

**Objective:** Disable direct SSH root login on all app servers within the Stratos Datacenter.

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
# Switch to root for SSH configuration
sudo su -
```

#### Step 3: Edit SSH Configuration
```bash
# Backup original SSH config
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

# Edit SSH configuration file
vi /etc/ssh/sshd_config
```

#### Step 4: Modify Root Login Setting
```bash
# Find and change PermitRootLogin to no
# Change: #PermitRootLogin yes
# To: PermitRootLogin no
```

#### Step 5: Restart SSH Service
```bash
# Restart SSH service to apply changes
systemctl restart sshd

# Verify SSH service status
systemctl status sshd
```

### App Server 2 (stapp02)

#### Step 1: Connect to App Server 2
```bash
# SSH into App Server 2
ssh steve@stapp02.stratos.xfusioncorp.com
```

#### Step 2: Switch to Root User
```bash
# Switch to root for SSH configuration
sudo su -
```

#### Step 3: Edit SSH Configuration
```bash
# Backup original SSH config
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

# Edit SSH configuration file
vi /etc/ssh/sshd_config
```

#### Step 4: Modify Root Login Setting
```bash
# Find and change PermitRootLogin to no
# Change: #PermitRootLogin yes
# To: PermitRootLogin no
```

#### Step 5: Restart SSH Service
```bash
# Restart SSH service to apply changes
systemctl restart sshd

# Verify SSH service status
systemctl status sshd
```

### App Server 3 (stapp03)

#### Step 1: Connect to App Server 3
```bash
# SSH into App Server 3
ssh banner@stapp03.stratos.xfusioncorp.com
```

#### Step 2: Switch to Root User
```bash
# Switch to root for SSH configuration
sudo su -
```

#### Step 3: Edit SSH Configuration
```bash
# Backup original SSH config
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

# Edit SSH configuration file
vi /etc/ssh/sshd_config
```

#### Step 4: Modify Root Login Setting
```bash
# Find and change PermitRootLogin to no
# Change: #PermitRootLogin yes
# To: PermitRootLogin no
```

#### Step 5: Restart SSH Service
```bash
# Restart SSH service to apply changes
systemctl restart sshd

# Verify SSH service status
systemctl status sshd
```

### Verification Commands (Run on each server)
```bash
# Check SSH configuration
grep "PermitRootLogin" /etc/ssh/sshd_config

# Test SSH service configuration
sshd -t

# Verify SSH service is running
systemctl is-active sshd
```

---

## ✅ Validation Steps

1. ✅ SSH configuration backed up on all servers
2. ✅ PermitRootLogin set to 'no' on all servers
3. ✅ SSH service restarted successfully on all servers
4. ✅ Direct root SSH login disabled on all app serverss