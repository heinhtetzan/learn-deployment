# 1. Install PostgreSQL

Update packages and install:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
```

Check status:

```bash
sudo systemctl status postgresql
```

Enable auto start:

```bash
sudo systemctl enable postgresql
```

---

# 2. Login to PostgreSQL

PostgreSQL uses a system user called **postgres**.

Switch user:

```bash
sudo -i -u postgres
```

Open PostgreSQL shell:

```bash
psql
```

You should see:

```text
postgres=#
```

---

# 3. Create Database

Example:

```sql
CREATE DATABASE app_db;
```

List databases:

```sql
\l
```

---

# 4. Create User

Create a user with password:

```sql
CREATE USER app_user WITH PASSWORD 'StrongPassword123!';
```

Check users:

```sql
\du
```

---

# 5. Grant Permissions (Single Database)

Allow full access to one database:

```sql
GRANT ALL PRIVILEGES ON DATABASE app_db TO app_user;
```

Switch to database:

```sql
\c app_db
```

Give permissions to tables and schema:

```sql
GRANT ALL ON SCHEMA public TO app_user;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO app_user;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO app_user;
```

Enable future tables permissions:

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT ALL ON TABLES TO app_user;
```

---

# 6. Grant Permission for Everything (All Databases)

If needed:

```sql
ALTER USER app_user WITH SUPERUSER;
```

But **not recommended for production**.

---

# 7. Enable Public Access (Remote Connections)

By default PostgreSQL only allows **localhost**.

---

## Edit PostgreSQL config

```bash
sudo nano /etc/postgresql/*/main/postgresql.conf
```

Find:

```text
listen_addresses = 'localhost'
```

Change to:

```text
listen_addresses = '*'
```

---

# 8. Allow Remote Authentication

Edit:

```bash
sudo nano /etc/postgresql/*/main/pg_hba.conf
```

Add this line at the bottom:

```text
host    all             all             0.0.0.0/0               md5
```

Meaning:

```text
Allow all users from any IP with password authentication
```

For better security (recommended):

```text
host    all             all             YOUR_IP/32              md5
```

Example:

```text
host    all             all             103.25.xxx.xxx/32       md5
```

---

# 9. Restart PostgreSQL

```bash
sudo systemctl restart postgresql
```

---

# 10. Open Firewall Port

PostgreSQL uses port **5432**.

```bash
sudo ufw allow 5432
```

Check firewall:

```bash
sudo ufw status
```

---

# 11. Test Remote Connection

From local machine:

```bash
psql -h SERVER_IP -U app_user -d app_db
```

Example:

```bash
psql -h 129.212.233.78 -U app_user -d app_db
```

---

# 12. PostgreSQL Connection String (For Apps)

### Node.js / Next.js

```text
postgresql://app_user:password@localhost:5432/app_db
```

---

### Laravel (.env)

```text
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=app_db
DB_USERNAME=app_user
DB_PASSWORD=password
```

---

# 13. Production Security Best Practice

Do **NOT open PostgreSQL to the whole internet**.

Recommended architecture:

```text
Internet
   ↓
Nginx
   ↓
App Server (Node / Laravel)
   ↓
PostgreSQL (private)
```

Ports:

```text
80   HTTP
443  HTTPS
5432 internal only
```

---

# PostgreSQL vs MySQL (Quick Production Comparison)

| Feature         | PostgreSQL  | MySQL  |
| --------------- | ----------- | ------ |
| ACID compliance | Very strong | Good   |
| Complex queries | Excellent   | Good   |
| JSON support    | Excellent   | Good   |
| Scaling         | Strong      | Strong |
| Laravel support | Native      | Native |

Many modern systems use **PostgreSQL**.

