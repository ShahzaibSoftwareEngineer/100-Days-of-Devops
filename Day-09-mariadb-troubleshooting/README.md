# 🛢️ Day 9 of 100 Days of DevOps – KodeKloud Challenge: MariaDB Troubleshooting

Today, I diagnosed and resolved a **MariaDB service outage** on the Nautilus DB server (`stdb01`) and restored database connectivity for the Nautilus application. This lab reinforced the importance of **systematic troubleshooting**, log analysis, and database administration in production-like environments.

---

## 📖 What is MariaDB?

MariaDB is an open-source relational database management system (RDBMS) and a drop-in replacement for MySQL. It is widely used for:

- **Data storage** – manage structured application data efficiently  
- **Backend support** – powers web applications and enterprise services  
- **High Availability** – supports replication, clustering, and failover  
- **SQL querying** – enables structured queries, transactions, and indexing

**Key Points:**
- Open-source and widely adopted
- Fully compatible with MySQL clients and tools
- Managed via `systemd` on Linux servers
- Strong security, performance, and scalability features

---

## 🛠️ Step-by-Step Commands Performed

### Step 1: Connect to the Database Server
```bash
ssh peter@stdb01
```
Connected securely to the Nautilus DB server to start troubleshooting.

### Step 2: Check MariaDB Service Status
```bash
sudo systemctl status mariadb
```
Verified that MariaDB was inactive. Initial logs suggested permission issues during startup.

### Step 3: Start MariaDB Service
```bash
sudo systemctl start mariadb
```
Started the database service. Initially failed due to file access permissions; resolved by ensuring correct ownership for `/var/lib/mysql`.

### Step 4: Enable MariaDB to Start Automatically on Boot
```bash
sudo systemctl enable mariadb
```
Configured MariaDB to start automatically after a server reboot, preventing future downtime.

### Step 5: Inspect Logs if Service Fails
```bash
sudo journalctl -xeu mariadb.service
sudo tail -n 50 /var/log/mariadb/mariadb.log
```
Logs revealed InnoDB errors due to permission issues. Corrected directory access to allow proper initialization.

### Step 6: Log in to MariaDB as Root
```bash
sudo mysql -u root -p
```
Root access allowed performing administrative tasks like checking databases and user privileges.

### Step 7: Grant Remote Access to Application User
```sql
GRANT ALL PRIVILEGES ON *.* TO 'peter'@'%' IDENTIFIED BY 'Sp!dy';
FLUSH PRIVILEGES;
```
Allowed the Nautilus application to connect remotely from any host.

### Step 8: Test Connectivity from Application Server
```bash
mysql -h 172.16.239.10 -u peter -p
```
Verified that the application could connect to the database successfully.

---

## ✅ Key Takeaways

- **Service Status**: Always check `systemctl status` first to confirm service health
- **Log Analysis**: Essential for diagnosing permission or configuration issues
- **User Privileges**: Properly configured users and host permissions prevent remote connectivity errors
- **Production Mindset**: Stepwise troubleshooting reduces downtime and ensures reliability
- **Hands-On Experience**: Simulated real-world database administration and incident handling

