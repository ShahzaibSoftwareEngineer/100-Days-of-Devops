# 📜 Day 10 of 100 Days of DevOps – KodeKloud Challenge: Linux Bash Scripts

Today, I explored **Linux Bash scripting** and **task automation** using cron jobs. I created an automated backup script for an e-commerce project and scheduled it to run daily, demonstrating how DevOps engineers automate routine tasks to improve efficiency and reduce manual errors.

---

## 📚 What is Bash Scripting?

**Bash (Bourne Again Shell)** is a command-line interpreter and scripting language for Unix/Linux systems. Bash scripts enable automation of repetitive Linux tasks such as backups, log rotation, and system monitoring.

### Key Benefits:
- **Task Automation** – Eliminate repetitive manual work
- **Error Reduction** – Minimize human mistakes in routine tasks
- **Consistency** – Ensure tasks are performed the same way every time
- **Efficiency** – Save time by automating routine operations

---

## 🛠️ Lab Steps: E-commerce Backup Automation

### Step 1: Create the Backup Script
```bash
nano ecommerce_backup.sh
```

**Script Content:**
```bash
#!/bin/bash
# Script: ecommerce_backup.sh
# Description: Backup project data

tar -czf /home/user/backups/ecommerce_$(date +%F).tar.gz /home/user/ecommerce_project
echo "Backup completed on $(date)" >> /home/user/backups/backup.log
```

### Step 2: Set Script Permissions
```bash
chmod 755 ecommerce_backup.sh
```

### Step 3: Test the Script
```bash
./ecommerce_backup.sh
```
Check if the backup file and log are created in `/home/user/backups/`.

### Step 4: Schedule Script with Cron Job
```bash
crontab -e
```

Add the line to run the script daily at 2 AM:
```bash
0 2 * * * /home/user/ecommerce_backup.sh
```

### Step 5: Install Cron Service (if missing)
```bash
sudo yum install cronie -y
sudo systemctl enable --now crond
sudo systemctl status crond
```

### Step 6: Verify Cron Job
```bash
# List active cron jobs
crontab -l

# Check cron service logs
sudo tail -f /var/log/cron
```

---

## ✅ Key Points Learned

- **Automation Benefits**: Bash scripts save time and reduce human error
- **Cron Scheduling**: Essential for running automated tasks at specific intervals
- **Testing First**: Always test scripts manually before scheduling
- **File Permissions**: `chmod 755` is critical for script execution
- **Service Management**: Installing and enabling `cronie` ensures cron jobs run 

