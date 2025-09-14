

# 🔹 Day 6 of 100 Days of DevOps – KodeKloud Challenge | Create a Cron Job ⏰

## 📘 What is a Cron Job?

A **Cron Job** is a scheduled task in Linux that runs automatically at specified intervals.

* Managed by the **cron daemon (`crond`)**.
* Configured through the **crontab file**.
* Commonly used for **backups, monitoring, log rotation, and automation in DevOps pipelines**.

---

## ✅ Steps Completed

### 1️⃣ Connect to App Servers

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

### 2️⃣ Install `cronie` package

```bash
sudo yum install -y cronie
```

### 3️⃣ Start and Enable `crond` Service

```bash
sudo systemctl start crond
sudo systemctl enable crond
systemctl status crond
```

### 4️⃣ Add Cron Job for Root User

```bash
sudo crontab -e
```

Add the line:

```bash
*/5 * * * * echo hello > /tmp/cron_text
```

### 5️⃣ Verify Cron Job

```bash
sudo crontab -l
```

### 6️⃣ Wait and Check Output File (\~5 minutes)

```bash
sudo cat /tmp/cron_text
```

### 7️⃣ Check Cron Logs (for confirmation/debugging)

```bash
sudo grep CRON /var/log/cron
```

---

## 🔑 **Key Notes:**

↳ Cron job runs at every 5-minute interval.
↳ `crond` must be running for jobs to execute.
↳ Verified results via `/tmp/cron_text` and `/var/log/cron`.
↳ No reboot required; cron auto-starts on boot.

---

## 📝 Summary

Successfully tested a **Linux cron job** that automates task execution every 5 minutes.
This hands-on reinforced how **automation with cron** improves reliability, reduces manual effort, and is essential for **DevOps and SysAdmin workflows**.


