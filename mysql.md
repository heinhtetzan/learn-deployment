Here is a **simple production-ready MySQL setup guide** for Ubuntu VPS (like DigitalOcean). I will explain **install → create user → create database → permissions → allow public access**.

---

# 1. Install MySQL (Production Server)

```bash
sudo apt update
sudo apt install mysql-server -y
```

Check MySQL status:

```bash
sudo systemctl status mysql
```

Enable auto start:

```bash
sudo systemctl enable mysql
```

---

# 2. Secure MySQL (Recommended for Production)

Run:

```bash
sudo mysql_secure_installation
```

Recommended answers:

```
VALIDATE PASSWORD COMPONENT → Yes
Password policy → Medium
Remove anonymous users → Yes
Disallow root login remotely → Yes
Remove test database → Yes
Reload privilege tables → Yes
```

---

# 3. Login to MySQL

```bash
sudo mysql
```

or

```bash
mysql -u root -p
```

---

# 4. Create Database

Example database:

```sql
CREATE DATABASE app_db;
```

Check database list:

```sql
SHOW DATABASES;
```

---

# 5. Create User

Example user:

```sql
CREATE USER 'app_user'@'%' IDENTIFIED BY 'StrongPassword123!';
```

Explanation:

```
app_user → username
% → allow connection from any IP
```

For local only:

```sql
CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'StrongPassword123!';
```

---

# 6. Grant Permission (Single Database)

Give **all permissions for one database**:

```sql
GRANT ALL PRIVILEGES ON app_db.* TO 'app_user'@'%';
```

Apply changes:

```sql
FLUSH PRIVILEGES;
```

---

# 7. Grant Permission (All Databases)

If you want the user to access **everything**:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'app_user'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

---

# 8. Allow Public Access (Remote MySQL)

By default MySQL only allows **localhost**.

Edit config:

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Find:

```
bind-address = 127.0.0.1
```

Change to:

```
bind-address = 0.0.0.0
```

Save.

---

Restart MySQL:

```bash
sudo systemctl restart mysql
```

---

# 9. Open MySQL Port (3306)

If using **UFW firewall**:

```bash
sudo ufw allow 3306
```

Check firewall:

```bash
sudo ufw status
```

---

# 10. Test Remote Connection

From your local machine:

```bash
mysql -h SERVER_IP -u app_user -p
```

Example:

```bash
mysql -h 129.212.233.78 -u app_user -p
```

---

# 11. Check Users

Inside MySQL:

```sql
SELECT user,host FROM mysql.user;
```

---

# Production Security Tips

Do **NOT open MySQL to public unless needed**.

Better approach:

Allow only specific IP.

Example:

```sql
CREATE USER 'app_user'@'YOUR_IP' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON app_db.* TO 'app_user'@'YOUR_IP';
```

Example:

```
'103.xxx.xxx.xxx'
```

---

# Typical Production Setup (Best Practice)

```
App Server → MySQL Server (private network)

Node / Laravel / Go
        ↓
     MySQL
```

Ports exposed:

```
80  → HTTP
443 → HTTPS
3306 → internal only
```
