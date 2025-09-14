🚀 Day 2 of #100DaysOfDevOps — Temporary User Setup with Expiry



## 🎯 **Lab Overview**
**Objective:** Create a temporary user account with automatic expiry date  
**Environment:** Nautilus App Server 1 (stapp01)  
**User:** tony | **Target User:** john  
**Expiry Date:** March 28, 2024

---

## 🔍 **Key Concepts**

### **Temporary User Accounts**
- Automatically expire after specified date
- Used for contractors, interns, or temporary project access
- Improves security by preventing forgotten active accounts

### **Why Important in DevOps?**
- **Security:** Automatic access revocation without manual intervention
- **Compliance:** Meets audit requirements for time-limited access
- **Risk Management:** Prevents orphaned accounts in production systems

---

## 🛠️ **Implementation Steps**

### **Step 1: Connect to Server**
```bash
ssh tony@stapp01.stratos.xfusioncorp.com
```

### **Step 2: Create User with Expiry Date**
```bash
sudo useradd -e 2024-03-28 john
```

**Command Breakdown:**
- `useradd`: Creates new user account
- `-e 2024-03-28`: Sets account expiry date (YYYY-MM-DD format)
- `john`: Username for temporary account

### **Step 3: Verify User Creation**
```bash
id john
```

**Expected Output:**
```
uid=1003(john) gid=1003(john) groups=1003(john)
```

### **Step 4: Check Expiry Details**
```bash
sudo chage -l john
```

**Key Output:**
```
Account expires: Mar 28, 2024
```

### **Step 5: Test Account Access**
```bash
su - john
```

---

## ✅ **Validation Testing**

### **Verify Expiry Configuration**
```bash
# Check user in password database
grep john /etc/passwd

# View account expiry timestamp
sudo grep john /etc/shadow | cut -d: -f8
```

### **Post-Expiry Behavior**
After March 28, 2024, login attempts will fail with:
```
Your account has expired; please contact your system administrator
```

## 🔧 **Skills Practiced**
- Linux user lifecycle management
- Account expiry policy implementation
- System security automation
- Compliance-driven access control

---


*Critical skill for implementing secure, time-bound user access in DevOps environments.*