# Day 70: Configure Jenkins User Access ✅

## 🎯 Task Overview
Configured user access control in Jenkins using Project-based Matrix Authorization Strategy to ensure secure, role-based access for the development team.

---

## 📚 Theory: Jenkins User Access Control

### **What is Jenkins Authorization?**
Jenkins authorization determines what users can do after they've been authenticated. It controls access to:
- Overall Jenkins administration
- Individual jobs/projects
- Build execution
- Configuration changes

### **Authorization Strategies in Jenkins**

1. **Anyone can do anything** - No security (not recommended for production)
2. **Legacy mode** - Full access for authenticated users
3. **Logged-in users can do anything** - Requires login but no granular control
4. **Matrix-based security** - Fine-grained permission control
5. **Project-based Matrix Authorization Strategy** - Extends matrix-based security to individual projects ⭐

### **Why Project-based Matrix Authorization?**

This strategy allows you to:
- Set **global permissions** (overall Jenkins access)
- Set **per-project permissions** (specific job access)
- Implement **principle of least privilege**
- Control who can view, build, configure, or delete jobs

### **Permission Types**

**Overall Permissions:**
- **Administer** - Full control over Jenkins
- **Read** - View Jenkins dashboard
- **Configure** - Modify Jenkins settings

**Job Permissions:**
- **Read** - View job details and build history
- **Build** - Trigger builds
- **Configure** - Modify job settings
- **Delete** - Remove jobs
- **Workspace** - Access job workspace

---

## 🛠️ Lab Implementation

### **Objective**
Create a user with limited read-only access to Jenkins and a specific job while maintaining admin privileges.

### **Requirements**
- Username: `javed`
- Password: `YchZHRcLkL`
- Full Name: `Javed`
- Global Permission: Overall → Read only
- Job Permission: Read only on "Helloworld" job
- Admin retains full access
- Anonymous users have no access

---

## 📋 Step-by-Step Configuration

### **Step 1: Login to Jenkins**
```
URL: Jenkins Dashboard
Username: admin
Password: Adm!n321
```

### **Step 2: Create User**
1. Navigate to **Manage Jenkins** → **Manage Users**
2. Click **Create User**
3. Fill in details:
   - Username: `javed`
   - Password: `YchZHRcLkL`
   - Full name: `Javed`
   - Email: `javed@example.com`
4. Click **Create User**

### **Step 3: Install Matrix Authorization Plugin (if needed)**
1. Go to **Manage Jenkins** → **Manage Plugins**
2. Search for "Matrix Authorization Strategy Plugin"
3. Install and select "Restart Jenkins when installation is complete"
4. Wait for restart and login again

### **Step 4: Configure Global Security**
1. Navigate to **Manage Jenkins** → **Configure Global Security**
2. Under **Authorization**, select **Project-based Matrix Authorization Strategy**
3. Configure permission matrix:

**Permission Matrix Configuration:**
```
User/Group          | Overall-Administer | Overall-Read | Other Permissions
--------------------|--------------------|--------------|-----------------
admin               | ✅                 | ✅           | ✅ ALL
javed               | ❌                 | ✅           | ❌ None
Anonymous           | ❌                 | ❌           | ❌ None
```

4. Click **Save**

### **Step 5: Configure Job-Level Permissions**
1. Go to **Dashboard** → Click **"Helloworld"** job
2. Click **Configure**
3. Scroll to find **"Enable project-based security"**
4. ✅ **Check the box**
5. Configure job permission matrix:

**Job Permission Matrix:**
```
User/Group    | Build | Read | Configure | Delete | Other Job Permissions
--------------|-------|------|-----------|--------|----------------------
admin         | ✅    | ✅   | ✅        | ✅     | ✅ ALL
javed         | ❌    | ✅   | ❌        | ❌     | ❌ None
Anonymous     | ❌    | ❌   | ❌        | ❌     | ❌ None
```

6. Click **Save**

---

## ✅ Verification

### **Test User Access:**
1. Logout from admin
2. Login as `javed` / `YchZHRcLkL`
3. Verify:
   - ✅ Can see dashboard
   - ✅ Can view Helloworld job
   - ✅ Can see build history
   - ❌ Cannot see "Build Now" button
   - ❌ Cannot access "Manage Jenkins"
   - ❌ Cannot configure job

---

## 🔑 Key Concepts Learned

### **1. Principle of Least Privilege**
Users should only have the minimum permissions necessary to perform their tasks.

### **2. Defense in Depth**
- Global permissions control overall access
- Project-based permissions add another security layer
- Both levels must be configured correctly

### **3. Role-Based Access Control (RBAC)**
Different users have different roles:
- **Admin**: Full control
- **Developer (javed)**: Read-only access
- **Anonymous**: No access

### **4. Security Best Practices**
- ✅ Always remove anonymous access
- ✅ Use strong passwords
- ✅ Grant minimal necessary permissions
- ✅ Regularly audit user permissions
- ✅ Use project-based security for sensitive jobs

---