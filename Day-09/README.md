# 𝐃𝐚𝐲 𝟗 𝐨𝐟 𝟏𝟎𝟎 𝐃𝐚𝐲𝐬 𝐨𝐟 𝐃𝐞𝐯𝐎𝐏𝐒 – 𝐊𝐨𝐝𝐞𝐊𝐥𝐨𝐮𝐝 | 𝐌𝐚𝐫𝐢𝐀𝐃𝐁 𝐓𝐫𝐨𝐮𝐛𝐥𝐞𝐬𝐡𝐨𝐨𝐭𝐢𝐧𝐠

## 📝 Lab Objective
- Diagnose and resolve a MariaDB service outage.
- Restore connectivity for the Nautilus application.
- Apply best practices for database troubleshooting in production environments.

---

## 🔹 Scenario
The Nautilus application in Stratos DC reported **database connectivity issues**.  
The production support team identified that the **MariaDB service was down** on the database server (`stdb01`).  
In real-world production environments, database outages can halt application functionality and impact business operations. This lab simulates a **high-pressure troubleshooting scenario**.

---

## 🔹 Real-World Relevance
- **Database availability** is critical for application uptime.  
- **Troubleshooting skills** help minimize downtime during incidents.  
- Understanding **user privileges** and **network access** prevents connectivity errors.  
- Familiarity with **systemd and logs** allows quick diagnostics of service failures.  

---

## 🔧 Lab Tasks
1. Connect to the database server (`stdb01`).  
2. Check the MariaDB service status.  
3. Start the MariaDB service if it is down.  
4. Enable MariaDB to start automatically on boot.  
5. Inspect logs if the service fails to start.  
6. Log in as root to MariaDB.  
7. Grant remote access to the application DB user.  
8. Test connectivity from the application server (`stdb01`).  

---

## 🔧 Steps Performed

### 1. Connect to the database server
```bash
ssh peter@stdb01
Theory: SSH allows secure remote management of servers.

2. Check MariaDB status
bash
Copy code
sudo systemctl status mariadb
Theory: Confirms whether the service is running. Checks for inactive or failed status and systemd messages.

3. Start MariaDB service
bash
Copy code
sudo systemctl start mariadb
Theory: Restarts the service. In production, this is the first step after identifying a downed service.

4. Enable MariaDB on boot
bash
Copy code
sudo systemctl enable mariadb
Theory: Ensures automatic startup after reboot to prevent downtime.

5. Check logs if service fails
bash
Copy code
sudo journalctl -xeu mariadb.service
sudo tail -n 50 /var/log/mariadb/mariadb.log
Theory: Logs provide insight into permission issues, corrupted files, or misconfigurations that prevent MariaDB from starting.

6. Log in as root
bash
Copy code
sudo mysql -u root -p
Theory: Root access is required for administrative tasks such as creating users, managing privileges, and checking databases.

7. Grant remote access to the application user
sql
Copy code
GRANT ALL PRIVILEGES ON *.* TO 'peter'@'%' IDENTIFIED BY 'Sp!dy';
FLUSH PRIVILEGES;
Theory: By default, MariaDB users can only connect locally. Granting privileges with % allows remote connections from the application server.

8. Test connectivity from the application server
bash
Copy code
mysql -h 172.16.239.10 -u peter -p
Theory: Verifies that the application can successfully connect to the database after configuration changes.

✅ Key Learnings
Service Status: Always check systemctl status before restarting.

Logs Analysis: Critical for diagnosing startup failures and permission errors.

User Privileges: Properly configured database users prevent remote connectivity issues.

Production Mindset: Systematic troubleshooting reduces downtime and improves reliability.

Hands-On Practice: Simulates real-world incident handling and database administration.

