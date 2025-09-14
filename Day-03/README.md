

# 🚀 Day 3: Secure Root SSH Access

## 🔐 Overview

As part of Linux server hardening, it’s a security best practice to **disable direct root SSH login**.
Allowing root to log in directly over SSH increases the risk of brute-force attacks and unauthorized access.

Instead, users should log in with a **regular account** and escalate privileges using `sudo` when administrative tasks are required.

To implement this, we update the SSH daemon configuration file (`/etc/ssh/sshd_config`) and restart the SSH service.

---

## 🛠️ Steps Performed

### 1️⃣ SSH into Each App Server

```bash
# From jump host
ssh tony@stapp01     # Password: Ir0nM@n
ssh steve@stapp02    # Password: Am3ric@
ssh banner@stapp03   # Password: BigGr33n
```

---

### 2️⃣ Open SSH Configuration File

```bash
sudo vi /etc/ssh/sshd_config
```

---

### 3️⃣ Locate and Update Root Login Setting

Find the line with `PermitRootLogin`.

* If commented (`#PermitRootLogin yes`), uncomment it.
* Update the value to **no**:

```bash
PermitRootLogin no
```

---

### 4️⃣ Restart SSH Service

```bash
sudo systemctl restart sshd
```

---

### 5️⃣ Verify Changes

Run:

```bash
sudo grep -i PermitRootLogin /etc/ssh/sshd_config
```

✅ Expected Output:

```
PermitRootLogin no
# the setting of "PermitRootLogin without-password".
```

---

## ⚠️ Challenge Encountered

🔍 On **stapp02**, the SSH config file appeared empty.
🛠️ After careful troubleshooting, I reapplied the correct configuration.
✅ Successfully restarted the SSH service without issues.

---

## 🎯 Outcome

* Direct **root SSH login disabled** on all 3 app servers (`stapp01`, `stapp02`, `stapp03`).
* Enhanced server security by following industry best practices.
* Learned real-world debugging skills with SSH services.

