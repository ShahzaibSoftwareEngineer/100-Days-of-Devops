# 𝐃𝐚𝐲 𝟗 𝐨𝐟 𝟏𝟎𝟎 𝐃𝐚𝐲𝐬 𝐨𝐟 𝐃𝐞𝐯𝐎𝐏𝐒 – 𝐊𝐨𝐝𝐞𝐊𝐥𝐨𝐮𝐝 𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞: MariaDB Troubleshooting

Today, I diagnosed and resolved a **MariaDB service outage** on the Nautilus DB server (`stdb01`) and restored database connectivity for the Nautilus application. This lab reinforced the importance of **systematic troubleshooting** and **database administration skills** in real-world production environments.

---

## 📖 What is MariaDB?

MariaDB is an open-source relational database management system (RDBMS) and a **drop-in replacement for MySQL**. It is widely used for:

- **Data storage** – manage structured data efficiently.
- **Application backend support** – serves as a backend database for web and enterprise applications.
- **Replication & High Availability** – supports clustering and failover.
- **SQL querying** – allows data retrieval, updates, and transaction management.

**Key Points:**

- Open-source and community-driven.
- Compatible with MySQL client libraries and tools.
- Offers strong security, performance, and scalability features.
- Can be managed using `systemd` services on Linux.

---

## 🛠️ Step-by-Step Commands Performed

### Step 1: Connect to the Database Server
```bash
ssh peter@stdb01
Connected securely to the Nautilus DB server to begin troubleshooting.

Step 2: Check MariaDB Service Status
bash
Copy code
sudo systemctl status mariadb
Confirmed the MariaDB service was inactive. Systemd logs indicated a startup failure.

Step 3: Start MariaDB Service
bash
Copy code
sudo systemctl start mariadb
Restarted the database service. Initially failed due to permission issues; resolved by ensuring proper directory ownership.

Step 4: Enable MariaDB to Start on Boot
bash
Copy code
sudo systemctl enable mariadb
Ensured MariaDB would automatically start on server reboot, preventing future downtime.

Step 5: Check Logs if Service Fails
bash
Copy code
sudo journalctl -xeu mariadb.service
sudo tail -n 50 /var/log/mariadb/mariadb.log
Verified InnoDB initialization errors were due to permission issues on data directories.

Step 6: Log in as Root
bash
Copy code
sudo mysql -u root -p
Gained administrative access to MariaDB for user management and troubleshooting.

Step 7: Grant Remote Access to Application User
sql
Copy code
GRANT ALL PRIVILEGES ON *.* TO 'peter'@'%' IDENTIFIED BY 'Sp!dy';
FLUSH PRIVILEGES;
Allowed the Nautilus application to connect remotely to the database server.

Step 8: Test Connectivity from Application Server
bash
Copy code
mysql -h 172.16.239.10 -u peter -p
Connection successful. The Nautilus application could now access the database without issues.

✅ Key Takeaways
Service Management: Always check the service status with systemctl status before taking action.

Logs Analysis: Systemd and MariaDB logs help diagnose permission or configuration issues.

User Privileges: Correct privileges and host access are essential for remote database connectivity.

Production Mindset: Systematic troubleshooting reduces downtime and ensures business continuity.

Hands-On Experience: Practical exposure to real-world database issues strengthens DevOps and SysAdmin skills.