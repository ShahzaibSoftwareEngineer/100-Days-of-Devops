# 🚀 Day 1 of 100 Days of DevOps: Mastering Linux User Security

## 🎯 Challenge: 100 Days of DevOps with KodeKloud


## 🖥️ Server Details
| Server Name | IP Address | Hostname | User | Purpose |
|-------------|------------|----------|------|---------|
| stapp01 | 172.16.238.10 | stapp01.stratos.xfusioncorp.com | tony | Nautilus App 1 |

---

## 📋 Lab Overview
**Scenario:** xFusionCorp Industries system admin team needs to create a user with a non-interactive shell to accommodate backup agent tool specifications.

**Objective:** Create a user named `javed` with a non-interactive shell on App Server 1.

---

## 🔧 Step-by-Step Solution

### Step 1: Connect to App Server 1
```bash
# SSH into App Server 1 (stapp01 - Nautilus App 1)
ssh tony@stapp01.stratos.xfusioncorp.com
# OR using IP address
ssh tony@172.16.238.10

# Enter password when prompted: Ir0nM@n
```

### Step 2: Switch to Root User
```bash
# Switch to root for user management operations
sudo su -
```

### Step 3: Create User with Non-Interactive Shell
```bash
# Create user 'javed' with /sbin/nologin as shell
useradd -s /sbin/nologin javed
```

**Alternative command options:**
```bash
# Option 1: Using /bin/false as non-interactive shell
useradd -s /bin/false javed

# Option 2: Using /usr/sbin/nologin (on some systems)
useradd -s /usr/sbin/nologin javed
```

### Step 4: Verify User Creation
```bash
# Check if user was created successfully
id javed

# Verify the shell assignment
grep javed /etc/passwd

# Check user's home directory
ls -la /home/ | grep javed
```)

---

## 📖 Key Concepts Learned

### Non-Interactive Shells
- **Purpose:** Prevent users from logging into the system while allowing services/processes to run under their account
- **Common non-interactive shells:**
  - `/sbin/nologin` - Most common, displays message when login attempted
  - `/bin/false` - Simply returns false, no message
  - `/usr/sbin/nologin` - Alternative path on some systems

### useradd Command Options
- `-s` : Specify the login shell
- `-d` : Set home directory
- `-m` : Create home directory
- `-g` : Set primary group

---

## 🛠️ Commands Reference

| Command | Description |
|---------|-------------|
| `useradd -s /sbin/nologin javed` | Create user with non-interactive shell |
| `id javed` | Display user and group IDs |
| `grep javed /etc/passwd` | Check user entry in passwd file |
| `grep nologin /etc/passwd` | List all users with nologin shell |

---

## ✅ Validation Steps

1. ✅ User `javed` created successfully
2. ✅ User has non-interactive shell (`/sbin/nologin`)
3. ✅ User entry exists in `/etc/passwd`
4. ✅ User cannot login interactively



---

**#100DaysOfDevOps #KodeKloud #Linux #UserManagement #DevOps**