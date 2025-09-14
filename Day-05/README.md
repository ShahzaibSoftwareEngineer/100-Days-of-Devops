P

# 📘 Day 5: SELinux Installation and Configuration

Today, I worked on **Security-Enhanced Linux (SELinux)** — a security architecture for Linux systems developed by the **NSA (National Security Agency)**. SELinux is a kernel security module that provides **Mandatory Access Control (MAC)** in addition to the traditional **Discretionary Access Control (DAC)**.

---

## 🔹 SELinux 

* **What is SELinux?**
  SELinux is a Linux kernel security module that enforces security policies using **labels** on files, processes, and ports.

* **Why SELinux?**
  Traditional Linux permissions (user, group, others) are not enough for advanced security. SELinux prevents processes from doing things they are not explicitly allowed to, even if the standard permissions allow it.

* **Modes of SELinux**:

  1. **Enforcing** → Default mode, SELinux policy is applied, violations are blocked.
  2. **Permissive** → SELinux logs violations but does not block them (useful for troubleshooting).
  3. **Disabled** → SELinux completely turned off (not recommended).

* **Policy Types**:

  * **Targeted policy (default)** → Applies rules to specific targeted services (like Apache, SSH).
  * **MLS policy** → Multi-Level Security for very high-security environments.

* **SELinux Context**:
  Every file, process, and port is labeled with a **context** (user\:role\:type\:level). Example:

  ```
  system_u:object_r:httpd_sys_content_t:s0
  ```

  Here `httpd_sys_content_t` defines what Apache can access.

---

## ⚙️ Lab – SELinux Installation and Configuration

### 🔹 Step 1: Check SELinux status

```bash
sestatus
getenforce
```

### 🔹 Step 2: Install SELinux (if not installed)

For **RHEL/CentOS**:

```bash
sudo yum install selinux-policy selinux-policy-targeted policycoreutils -y
```

For **Ubuntu/Debian**:

```bash
sudo apt install selinux selinux-basics selinux-policy-default auditd -y
```

### 🔹 Step 3: Enable SELinux at Boot

Edit the config file:

```bash
sudo vi /etc/selinux/config
```

Set:

```
SELINUX=enforcing
SELINUXTYPE=targeted
```

### 🔹 Step 4: Switch Modes (Temporary)

```bash
sudo setenforce 1   # Enforcing
sudo setenforce 0   # Permissive
```

### 🔹 Step 5: View Loaded Policies

```bash
semodule -l
```

### 🔹 Step 6: Check File Contexts

```bash
ls -Z /var/www/html/
```

### 🔹 Step 7: Restore Default SELinux Context

```bash
sudo restorecon -Rv /var/www/html/
```

### 🔹 Step 8: Manage Ports in SELinux

```bash
# List SELinux ports
semanage port -l | grep http

# Allow Apache on custom port 8080
sudo semanage port -a -t http_port_t -p tcp 8080
```

### 🔹 Step 9: Troubleshooting with Audit Logs

```bash
# View SELinux denied actions
sudo ausearch -m avc --success no | less

# Generate human-readable report
sudo sealert -a /var/log/audit/audit.log
```

---

## 📝 Summary

* SELinux adds a **powerful security layer** by controlling access at the kernel level.
* It works using **labels (contexts)** and **policies** to enforce strict access rules.
* Modes: **Enforcing (active), Permissive (logs only), Disabled (off)**.
* Tools like `sestatus`, `semanage`, `restorecon`, and `audit2allow` are key in managing SELinux.
* Best practice: Always run SELinux in **Enforcing mode** in production systems.


