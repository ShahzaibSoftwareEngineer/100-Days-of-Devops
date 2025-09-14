
# 🚀 Day 4: Script Execution Permissions

### 📌 Lab Objective

Grant executable permissions to the `xfusioncorp.sh` script located in `/tmp` on **App Server 1**, ensuring that **all users** can execute it.

---

### 📖 Understanding Script Permissions

In Linux, permissions define what actions users can perform on a file:

* **Read (r):** View file contents
* **Write (w):** Modify the file
* **Execute (x):** Run the file as a program

By default, newly distributed scripts may lack execute permissions. Without them, the system cannot run the script.

* `chmod a+x file` → Adds execute permission for **all users**.
* `chmod 755 file` → A safer option: **owner has full control, group & others have read + execute**.

---

### 🛠️ Lab Steps

**1️⃣ Connect to App Server 1**

```bash
ssh tony@stapp01
```

**2️⃣ Check the script’s permissions**

```bash
ls -l /tmp/xfusioncorp.sh
```

Output:

```
---------- 1 root root 40 Aug  7 09:38 /tmp/xfusioncorp.sh
```

➡️ The script had **no read, write, or execute permissions**.

---

### ❌ Error Encountered

I tried:

```bash
sudo chmod a+x /tmp/xfusioncorp.sh
/tmp/xfusioncorp.sh
```

But got:

```
/bin/bash: /tmp/xfusioncorp.sh: Permission denied
```

🔍 **Reason:**
The script only had execute bits (`--x--x--x`) but **no read permission**, so the shell couldn’t read the file.

---

### ✅ Fix Applied

I corrected the permissions with:

```bash
sudo chmod 755 /tmp/xfusioncorp.sh
```

**Verify again:**

```bash
ls -l /tmp/xfusioncorp.sh
```

Now:

```
-rwxr-xr-x 1 root root 40 Aug  7 09:38 /tmp/xfusioncorp.sh
```

---

### 🚀 Result

Executed successfully:

```bash
/tmp/xfusioncorp.sh
```

The script ran without errors after applying the proper permissions.

---

### 🎯 Key Takeaways

* Always check permissions with `ls -l`.
* `chmod a+x` alone may not be enough — scripts often also need **read access**.
* Use `chmod 755` as a standard for shared scripts.
* Proper permission management is critical in DevOps automation.

---


