# 1. What is Redis

**Redis** = In-memory key-value database.

Meaning:

```text
Data stored in RAM → extremely fast
```

Used for:

* caching
* sessions
* queues
* rate limiting
* pub/sub
* real-time systems

Example flow:

```text
App → Redis → MySQL
```

Redis reduces database load.

---

# 2. Install Redis (Ubuntu Production Server)

```bash
sudo apt update
sudo apt install redis-server -y
```

Check status:

```bash
sudo systemctl status redis
```

Enable auto start:

```bash
sudo systemctl enable redis
```

---

# 3. Test Redis

Run:

```bash
redis-cli
```

Test:

```text
PING
```

Response:

```text
PONG
```

Example key:

```text
SET name "Ko Htet"
GET name
```

Output:

```text
Ko Htet
```

Exit:

```text
exit
```

---

# 4. Production Configuration

Edit config:

```bash
sudo nano /etc/redis/redis.conf
```

---

## Important Settings

### Bind address

Default:

```text
bind 127.0.0.1
```

Allow remote access:

```text
bind 0.0.0.0
```

---

### Protected mode

Find:

```text
protected-mode yes
```

For public server change to:

```text
protected-mode no
```

---

# 5. Add Password (Important)

Find:

```text
# requirepass foobared
```

Change to:

```text
requirepass StrongRedisPassword123
```

Now Redis requires authentication.

---

# 6. Restart Redis

```bash
sudo systemctl restart redis
```

---

# 7. Connect with Password

```bash
redis-cli
```

Login:

```text
AUTH StrongRedisPassword123
```

Example:

```text
SET hello world
GET hello
```

---

# 8. Open Redis Port

Redis port:

```text
6379
```

Open firewall:

```bash
sudo ufw allow 6379
```

Check:

```bash
sudo ufw status
```

---

# 9. Test Remote Connection

From another machine:

```bash
redis-cli -h SERVER_IP -p 6379
```

Example:

```bash
redis-cli -h 129.212.233.78 -p 6379
```

Authenticate:

```text
AUTH StrongRedisPassword123
```

---

# 10. Redis for Node.js / Next.js

Install client:

```bash
npm install redis
```

Example:

```javascript
import { createClient } from "redis"

const client = createClient({
  url: "redis://:password@localhost:6379"
})

await client.connect()

await client.set("name", "Ko Htet")

const value = await client.get("name")

console.log(value)
```

---

# 11. Redis for Laravel

Install driver:

```bash
composer require predis/predis
```

`.env`

```text
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=StrongRedisPassword123
REDIS_PORT=6379
```

Example use:

```php
Cache::put('name', 'Ko Htet', 60);
Cache::get('name');
```

---

# 12. Redis Production Uses

Typical architecture:

```text
Internet
   ↓
Nginx
   ↓
Next.js / Node / Laravel
   ↓
Redis (cache)
   ↓
MySQL / PostgreSQL
```

Example caching:

```text
User request
     ↓
Check Redis
     ↓
Cache hit → return data
Cache miss → query database
```

---

# 13. Enable Redis Persistence (Important)

Redis is RAM-based but can save data to disk.

Edit config:

```bash
sudo nano /etc/redis/redis.conf
```

Enable:

```text
appendonly yes
```

This enables **AOF persistence**.

Restart:

```bash
sudo systemctl restart redis
```

---

# 14. Redis Monitoring

Check memory:

```bash
redis-cli INFO memory
```

Check clients:

```bash
redis-cli INFO clients
```

Monitor commands:

```bash
redis-cli monitor
```

---

# 15. Production Security (Very Important)

Do NOT expose Redis publicly without security.

Recommended:

```text
bind 127.0.0.1
```

or private network.

Example production architecture:

```text
App Server → Redis → MySQL
```

Ports:

```text
80   HTTP
443  HTTPS
6379 internal only
```

---

# 16. Real Production Stack (Modern Backend)

Example used by many companies:

```text
Cloudflare
     ↓
Nginx
     ↓
Next.js (PM2 cluster)
     ↓
Node / Laravel API
     ↓
Redis cache
     ↓
MySQL / PostgreSQL
```

Redis improves:

* speed
* scalability
* database performance