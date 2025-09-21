# 🚀 Day 4 of 100 Days of DevOps: Script Execution Permissions

## 🎯 Challenge: 100 Days of DevOps with KodeKloud

## 🖥️ Server Details
| Server Name | IP Address | Hostname | User | Purpose |
|-------------|------------|----------|------|---------|
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Nautilus App 1 |

---

## 📋 Lab Overview
**Scenario:** xFusionCorp Industries sysadmin team has developed a bash script named `xfusioncorp.sh` to automate backup processes. The script lacks executable permissions on App Server 1.

**Objective:** Grant executable permissions to `/tmp/xfusioncorp.sh` script on App Server 1 and ensure all users can execute it.

---

## 🔧 Step-by-Step Solution

### Step 1: Connect to App Server 1
```bash
# SSH into App Server 1 (stapp01 - Nautilus App 1)
ssh tony@stapp01.stratos.xfusioncorp.com
# OR using IP address
ssh tony@172.16.238.10
```

### Step 2: Switch to Root User
```bash
# Switch to root for file permission changes
sudo su -
```

### Step 3: Check Current Script Permissions
```bash
# Check current permissions of the script
ls -la /tmp/xfusioncorp.sh

# View detailed file information
stat /tmp/xfusioncorp.sh
```

### Step 4: Grant Executable Permissions for All Users
```bash
# Method 1: Add execute permission for all users (owner, group, others)
chmod +x /tmp/xfusioncorp.sh

# Method 2: Set specific permissions (read and execute for all)
chmod 755 /tmp/xfusioncorp.sh

# Method 3: Using symbolic notation
chmod a+x /tmp/xfusioncorp.sh
```

### Step 5: Verify Permissions
```bash
# Check updated permissions
ls -la /tmp/xfusioncorp.sh

# Verify all users can execute
stat /tmp/xfusioncorp.sh
```

### Step 6: Test Script Execution
```bash
# Test script execution as current user
/tmp/xfusioncorp.sh

# Check if script runs without permission errors
bash /tmp/xfusioncorp.sh
```

---

## ✅ Validation Steps

1. ✅ Script `/tmp/xfusioncorp.sh` exists on App Server 1
2. ✅ Execute permissions granted for all users (owner, group, others)
3. ✅ File permissions show executable bits set (-rwxr-xr-x)
4. ✅ Script can be executed by all users without permission errors