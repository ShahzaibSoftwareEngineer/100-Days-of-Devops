
# 🐳 Day 46 of #100DaysOfDevOps – Deploying a PHP + MariaDB App with Docker Compose

## 🎯 Objective
The goal of this lab is to deploy a multi-container application stack using **Docker Compose**.  
We run:
- **MariaDB (mysql_host)** → for the  database backend  
- **PHP + Apache (php_host)** → for the web frontend  

---

## 🛠️ Lab Environment
- App Server: **stapp03.stratos.xfusioncorp.com**
- Docker: Installed and running
- User: `banner` with sudo access

---

## 📂 Project Files

### docker-compose.yml
```yaml
services:
  mysql_host:
    image: mariadb:latest
    container_name: mysql_host
    environment:
      MYSQL_ROOT_PASSWORD: Root@Pass123
      MYSQL_DATABASE: database_host
      MYSQL_USER: app_user
      MYSQL_PASSWORD: AppUser@Pass456!
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql

  php_host:
    build: .
    container_name: php_host
    ports:
      - "8089:80"
    volumes:
      - ./var-www-html:/var/www/html
    depends_on:
      - mysql_host

volumes:
  db_data:
````

---

### Dockerfile

```dockerfile
FROM php:apache

# Install PHP extensions
RUN docker-php-ext-install mysqli pdo pdo_mysql
```

---

### var-www-html/index.php

```php
<?php
echo "🚀 PHP + Apache container is working!";
?>
```

---

### var-www-html/db\_test.php

```php
<?php
$conn = new mysqli('mysql_host', 'app_user', 'AppUser@Pass456!', 'database_host');
if ($conn->connect_error) {
    die('Connection failed: ' . $conn->connect_error);
}
echo '✅ Database connection successful!';
?>
```

---

## 📜 Step-by-Step Commands

### 1. Clone repo and move into directory

```bash
cd ~/devops
```

### 2. Build and run containers

```bash
sudo docker compose up -d
```

### 3. Verify running containers

```bash
sudo docker ps
```

**Output:**

```
CONTAINER ID   IMAGE            COMMAND                  STATUS          PORTS
3c9d324c44ec   php:apache       "docker-php-entrypoi…"   Up              0.0.0.0:8089->80/tcp     php_host
97c96d4c366f   mariadb:latest   "docker-entrypoint.s…"   Up              0.0.0.0:3306->3306/tcp   mysql_host
```

---

### 4. Enter MariaDB container

```bash
sudo docker exec -it mysql_host mysql -uapp_user -pAppUser@Pass456! database_host
```

### 5. Database operations

```sql
SHOW DATABASES;
USE database_host;

CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    role VARCHAR(50)
);

INSERT INTO employees (name, role) VALUES
('Alice', 'Developer'),
('Bob', 'DevOps Engineer');

SELECT * FROM employees;
```

**Output:**

```
+----+-------+-----------------+
| id | name  | role            |
+----+-------+-----------------+
|  1 | Alice | Developer       |
|  2 | Bob   | DevOps Engineer |
+----+-------+-----------------+
```

---

### 6. Test PHP container

```bash
sudo docker exec -it php_host bash
cd /var/www/html
```

Create PHP DB test file:

```bash
echo "<?php
\$conn = new mysqli('mysql_host', 'app_user', 'AppUser@Pass456!', 'database_host');
if (\$conn->connect_error) {
    die('Connection failed: ' . \$conn->connect_error);
}
echo '✅ Database connection successful!';
?>" > db_test.php
```

---

### 7. Test application

```bash
curl http://localhost:8089/index.php
```

Output:

```
🚀 PHP + Apache container is working!
```

```bash
curl http://localhost:8089/db_test.php
```

Output:

```
✅ Database connection successful!
```

---

## 🧾 Conclusion

* Deployed **multi-container app** using Docker Compose
* Connected **PHP-Apache frontend** with **MariaDB backend**
* Successfully verified **database connectivity** with test scripts

---


