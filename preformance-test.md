## Testing Server Performance with **Apache JMeter**

**Apache JMeter** is a popular **load testing and performance testing tool** used to simulate many users hitting your server at the same time.

Developers often use it to test:

* **API performance**
* **Web application scalability**
* **Server stress limits**
* **Database bottlenecks**

For example, if you are running **Laravel APIs** or a **Next.js frontend** behind **Nginx on your VPS**, JMeter can simulate **hundreds or thousands of users** hitting your endpoints.

---

# 1. Install JMeter

### Mac (using Homebrew)

```bash
brew install jmeter
```

Run:

```bash
jmeter
```

---

### Ubuntu

```bash
sudo apt update
sudo apt install jmeter
```

Run:

```bash
jmeter
```

---

# 2. Create a Basic Load Test

Steps inside JMeter GUI:

### 1️⃣ Create Test Plan

```
Test Plan
   └── Thread Group
```

Right click:

```
Test Plan → Add → Threads → Thread Group
```

Configure:

| Setting         | Example |
| --------------- | ------- |
| Threads (users) | 100     |
| Ramp-Up         | 10 sec  |
| Loop Count      | 10      |

Meaning:

```
100 users
start within 10 seconds
each user sends 10 requests
```

---

# 3. Add HTTP Request

Right click **Thread Group**

```
Add → Sampler → HTTP Request
```

Example API test:

| Field    | Value           |
| -------- | --------------- |
| Protocol | http            |
| Server   | api.example.com |
| Method   | GET             |
| Path     | /api/products   |

Example:

```
GET https://api.example.com/api/products
```

---

# 4. Add Result Listeners

To view results:

```
Thread Group
   ├── HTTP Request
   └── Listener → View Results Tree
```

Useful listeners:

| Listener          | Purpose             |
| ----------------- | ------------------- |
| View Results Tree | debug               |
| Summary Report    | performance summary |
| Graph Results     | visual charts       |
| Aggregate Report  | production metrics  |

---

# 5. Run the Test

Click **Start (▶)**.

JMeter will simulate traffic.

Example output:

| Metric            | Meaning       |
| ----------------- | ------------- |
| Throughput        | requests/sec  |
| Avg Response Time | server speed  |
| Error %           | failure rate  |
| Latency           | network delay |

---

# 6. Example Performance Result

Example result:

| Metric       | Value  |
| ------------ | ------ |
| Users        | 500    |
| Requests/sec | 320    |
| Avg Response | 120 ms |
| Errors       | 0.5%   |

Meaning:

```
Server handles ~320 requests/sec
```

---

# 7. CLI Load Testing (Production Style)

GUI is **not recommended for large tests**.

Use CLI:

```bash
jmeter -n \
-t test-plan.jmx \
-l result.jtl
```

Options:

| Flag | Meaning      |
| ---- | ------------ |
| -n   | non-GUI mode |
| -t   | test plan    |
| -l   | result log   |

---

# 8. Example API Load Test for Laravel

Testing endpoint:

```
GET /api/products
```

Scenario:

```
500 users
ramp-up 30 seconds
loop 20
```

Total requests:

```
500 × 20 = 10,000 requests
```

Good for testing:

* Laravel queue performance
* database queries
* Redis caching
* Nginx limits

---

# 9. Important Metrics to Watch

When testing your server:

### CPU

```bash
htop
```

### Memory

```bash
free -m
```

### Network

```bash
iftop
```

### Nginx logs

```bash
tail -f /var/log/nginx/access.log
```

---

# 10. Real Production Testing Flow

Typical workflow used by DevOps:

```
Local environment test
        ↓
Staging load test
        ↓
Database performance tuning
        ↓
Cache tuning
        ↓
Production load test
```

---

💡 **Tip for your setup** (since you run Laravel / Next.js on VPS):

A realistic test might be:

```
1000 concurrent users
API requests
frontend page loads
database queries
```

This helps you determine:

* VPS capacity
* optimal **Nginx worker settings**
* **PHP-FPM pool size**
* **database connection limits**


---

Since you’re working with **Laravel APIs**, **Next.js**, and VPS infrastructure with **Nginx**, here is a **more realistic performance testing guide** similar to what **DevOps teams use in production systems**.

---

# 1. Realistic JMeter Test Plan for Laravel APIs

A **real API test** is not just one endpoint.
It simulates **real user behavior**.

Example Laravel API:

```
POST /api/login
GET  /api/products
POST /api/cart
POST /api/checkout
```

### JMeter Test Plan Structure

```
Test Plan
   └── Thread Group
         ├── HTTP Request Defaults
         ├── HTTP Header Manager
         ├── Login Request
         ├── Get Products
         ├── Add to Cart
         ├── Checkout
         └── Listeners
```

---

### Example Thread Group

| Setting | Value  |
| ------- | ------ |
| Users   | 500    |
| Ramp-up | 60 sec |
| Loop    | 10     |

Meaning:

```
500 users
start gradually in 60 seconds
each performs 10 operations
```

Total requests:

```
500 × 10 × endpoints
```

---

### Example HTTP Request

Login API:

```
POST /api/login
```

Body:

```json
{
 "email": "test@example.com",
 "password": "password"
}
```

---

### Header Manager

Important for Laravel APIs:

```
Content-Type: application/json
Accept: application/json
```

For authenticated APIs:

```
Authorization: Bearer TOKEN
```

---

# 2. Simulating 10,000 Users

Testing **10k users from one laptop is unrealistic**.

Instead use **distributed load testing**.

Architecture:

```
Controller (JMeter)
      |
-----------------------------
|            |              |
Load Node1  Load Node2   Load Node3
```

Example:

| Node     | Users |
| -------- | ----- |
| Server 1 | 3000  |
| Server 2 | 3000  |
| Server 3 | 4000  |

Total:

```
10,000 virtual users
```

Run command:

```
jmeter -n -t test.jmx -l result.jtl
```

---

# 3. How Companies Test 50k–100k Users

Large systems use **layered performance testing**.

### Example SaaS system architecture

```
Users
   |
CDN
   |
Load Balancer
   |
Nginx Cluster
   |
Application Servers
   |
Database Cluster
```

Testing strategy:

| Test Type   | Purpose                |
| ----------- | ---------------------- |
| Load Test   | normal traffic         |
| Stress Test | break the system       |
| Spike Test  | sudden traffic jump    |
| Soak Test   | long-running stability |

Example scenario:

```
Normal traffic = 10k users
Peak traffic = 50k users
Stress test = 100k users
```

Monitoring tools during test:

```
htop
iostat
netstat
docker stats
```

---

# 4. Better Modern Tools than JMeter

JMeter is powerful but **older**.

Many companies now prefer modern tools.

---

## k6

k6 is extremely popular in DevOps.

Advantages:

* JavaScript scripting
* very fast
* cloud integration
* good for CI/CD

Example test:

```javascript
import http from 'k6/http';

export default function () {
  http.get('https://api.example.com/products');
}
```

Run:

```
k6 run script.js
```

Example:

```
k6 run --vus 1000 --duration 30s script.js
```

Meaning:

```
1000 users
30 seconds
```

---

## Locust

Locust uses **Python scripts**.

Example:

```python
from locust import HttpUser, task

class MyUser(HttpUser):

    @task
    def products(self):
        self.client.get("/api/products")
```

Run:

```
locust
```

Open dashboard:

```
http://localhost:8089
```

---

## Artillery

Artillery is popular for **Node.js / API systems**.

Example config:

```yaml
config:
  target: "https://api.example.com"
  phases:
    - duration: 60
      arrivalRate: 50

scenarios:
  - flow:
      - get:
          url: "/api/products"
```

Run:

```
artillery run test.yml
```

---

# 5. Real DevOps Performance Testing Pipeline

Professional workflow:

```
Developer machine
        ↓
Local load test
        ↓
Staging environment
        ↓
Automated CI load test
        ↓
Production traffic simulation
```

Example CI pipeline:

```
GitHub Actions
      ↓
Build Docker
      ↓
Run k6 test
      ↓
Deploy if performance OK
```

---

# 6. Example Real Metrics (Laravel API)

Example result:

```
Users: 5000
Requests/sec: 2200
Average latency: 180ms
Error rate: 0.3%
CPU: 70%
Memory: 60%
```

This tells DevOps:

```
Server still has capacity
system stable
safe for production
```