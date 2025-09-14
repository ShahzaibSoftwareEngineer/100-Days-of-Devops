# 🚀 Day 1 of 100 Days of DevOps: Mastering Linux User Security

## 🎯 **Lab Overview**
**Objective:** Create a non-interactive user account on App Server 1 for secure system process management  
**Environment:** Nautilus App Server 1 (stapp01)  
**User:** tony | **Target User:** james

---

## 🔍 **Key Concepts**

### **Non-Interactive Shell (`/sbin/nologin`)**
- Prevents interactive login while allowing process execution
- Used for service accounts, backup agents, and automated processes
- Implements principle of least privilege for enhanced security

### **Why Important in DevOps?**
- **Security:** Reduces attack surface by preventing unauthorized shell access
- **Automation:** Enables secure automated processes without login capabilities
- **Compliance:** Meets enterprise security requirements for service accounts

---

## 🛠️ **Implementation Steps**

### **Step 1: Connect to Server**
```bash
ssh tony@stapp01
```

### **Step 2: Create Non-Interactive User**
```bash
sudo useradd -s /sbin/nologin james
```

**Command Breakdown:**
- `useradd`: Creates new user account
- `-s /sbin/nologin`: Sets non-interactive shell
- `james`: Username for new account

### **Step 3: Verify User Creation**
```bash
grep james /etc/passwd
```

**Expected Output:**
```
james:x:1002:1002::/home/james:/sbin/nologin
```

### **Step 4: Exit Server**
```bash
exit
```

---

## ✅ **Validation Testing**

### **Security Test (Should Fail)**
```bash
su - james
# Expected: "This account is currently not available."
```

### **Process Test (Should Work)**
```bash
sudo -u james whoami
# Expected: "james"
```

## 🔧 **Skills Practiced**
- Linux user management commands
- SSH remote server access
- Security hardening principles
- System authentication configuration

---

## 💡 **Key Takeaways**
- Non-interactive shells provide security without breaking functionality
- Service accounts should always use restricted shell access
- Proper user management is fundamental to DevOps security
- This pattern scales across containerized and cloud environments

---
