# 🚀 Day 7 of 100 Days of DevOps: Linux SSH Authentication

## 🎯 Challenge: 100 Days of DevOps with KodeKloud

## 🖥️ Server Details
| Server Name | IP Address | Hostname | User | Purpose |
|-------------|------------|----------|------|---------|
| jumphost | - | - | thor | Jump Server |
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Nautilus App 1 |
| stapp02 | 172.16.238.11 | stapp02.stratos.xfusioncorp.com | steve | Nautilus App 2 |
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | Nautilus App 3 |

---

## 📋 Lab Overview
**Scenario:** xFusionCorp Industries system admins team has set up scripts on jump host that run on regular intervals and perform operations on all app servers.

**Objective:** Set up password-less SSH authentication from user `thor` on jump host to all app servers through their respective sudo users.

---

## 🔹 SSH Key Authentication Overview

* **What is SSH Key Authentication?**
  SSH key authentication uses a pair of cryptographic keys (public and private) instead of passwords for secure authentication.

* **Why Use SSH Keys?**
  More secure than passwords, enables automation without manual password entry, prevents brute force attacks, and allows centralized key management.

---

## 🔧 Step-by-Step Solution

### Step 1: Generate SSH Key Pair on Jump Host
```bash
# Generate SSH key pair for thor user (run on jump host)
ssh-keygen -t rsa -b 4096

# Press Enter for default file location (~/.ssh/id_rsa)
# Press Enter twice for no passphrase (password-less access)
```

### Step 2: Verify SSH Key Generation
```bash
# Check generated keys
ls -la ~/.ssh/

# View public key content
cat ~/.ssh/id_rsa.pub

# View private key (keep this secure)
ls -la ~/.ssh/id_rsa
```

### Step 3: Copy Public Key to App Server 1
```bash
# Copy SSH public key to tony@stapp01
ssh-copy-id tony@stapp01.stratos.xfusioncorp.com

# Alternative method using IP
ssh-copy-id tony@172.16.238.10
```

### Step 4: Copy Public Key to App Server 2
```bash
# Copy SSH public key to steve@stapp02
ssh-copy-id steve@stapp02.stratos.xfusioncorp.com

# Alternative method using IP
ssh-copy-id steve@172.16.238.11
```

### Step 5: Copy Public Key to App Server 3
```bash
# Copy SSH public key to banner@stapp03
ssh-copy-id banner@stapp03.stratos.xfusioncorp.com

# Alternative method using IP
ssh-copy-id banner@172.16.238.12
```

### Step 6: Test Password-less SSH Access
```bash
# Test SSH access to App Server 1 (should not prompt for password)
ssh tony@stapp01.stratos.xfusioncorp.com

# Exit and test App Server 2
exit
ssh steve@stapp02.stratos.xfusioncorp.com

# Exit and test App Server 3
exit
ssh banner@stapp03.stratos.xfusioncorp.com
```

### Step 7: Verify SSH Configuration on Remote Servers
```bash
# On each app server, check authorized_keys file
cat ~/.ssh/authorized_keys

# Check SSH directory permissions
ls -la ~/.ssh/
```

---

## ✅ Validation Steps

1. ✅ SSH key pair generated on jump host for thor user
2. ✅ Public key copied to all three app servers
3. ✅ Password-less SSH access working to stapp01 (tony)
4. ✅ Password-less SSH access working to stapp02 (steve)
5. ✅ Password-less SSH access working to stapp03 (banner)