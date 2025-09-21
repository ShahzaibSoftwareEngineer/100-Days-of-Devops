# 🚀 Day 2 of 100 Days of DevOps: Temporary User Management

## 🎯 Challenge: 100 Days of DevOps with KodeKloud

## 🖥️ Server Details
| Server Name | IP Address | Hostname | User | Purpose |
|-------------|------------|----------|------|---------|
| stapp03 | 172.16.238.12 | stapp03.stratos.xfusioncorp.com | banner | Nautilus App 3 |

---

## 📋 Lab Overview
**Scenario:** A developer named `kareem` requires temporary access to the Nautilus project for a limited duration.

**Objective:** Create a user named `kareem` on App Server 3 with expiry date set to `2024-01-28`.

---

## 🔧 Step-by-Step Solution

### Step 1: Connect to App Server 3
```bash
# SSH into App Server 3 (stapp03 - Nautilus App 3)
ssh banner@stapp03.stratos.xfusioncorp.com
# OR using IP address
ssh banner@172.16.238.12

# Enter password when prompted: BigGr33n
```

### Step 2: Switch to Root User
```bash
# Switch to root for user management operations
sudo su -
```

### Step 3: Create User with Expiry Date
```bash
# Create user 'kareem' with expiry date 2024-01-28
useradd -e 2024-01-28 kareem
```

**Alternative command options:**
```bash
# Using different date format
useradd --expiredate 2024-01-28 kareem

# Create user first, then set expiry
useradd kareem
chage -E 2024-01-28 kareem
```

### Step 4: Verify User Creation and Expiry
```bash
# Check if user was created successfully
id kareem

# Verify user entry in passwd file
grep kareem /etc/passwd

# Check user's expiry date
chage -l kareem

# Alternative method to check expiry
passwd -S kareem
```

---

## ✅ Validation Steps

1. ✅ User `kareem` created successfully
2. ✅ User expiry date set to `2024-01-28`
3. ✅ User entry exists in `/etc/passwd`
4. ✅ User created in lowercase as per protocol