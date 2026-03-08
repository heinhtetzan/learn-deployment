## What is PM2?

**PM2** is a **production process manager for Node.js applications**.

A **process manager** is a tool that:

* starts your application
* keeps it running
* restarts it if it crashes
* manages multiple instances
* monitors performance

Officially developed by the Keymetrics team.

---

# 1️⃣ What PM2 Does

Without PM2:

```bash
node server.js
```

Problems:

* If the app crashes → it stops
* If the server reboots → app does not start
* No monitoring
* No clustering

With PM2:

```bash
pm2 start server.js
```

PM2 will:

✔ keep the app alive
✔ restart if crash
✔ auto start on reboot
✔ run multiple instances
✔ monitor CPU & memory

---

# 2️⃣ Why PM2 Exists

Node.js applications normally run like this:

```
Terminal
   ↓
node server.js
   ↓
App running
```

If you close the terminal:

```
App stops ❌
```

In **production servers**, apps must run **24/7**.

PM2 solves this by running apps as **background services**.

```
Server
   ↓
PM2
   ↓
Node Apps
```

PM2 becomes the **manager of your processes**.

---

# 3️⃣ Key Features of PM2

## 1. Process Management

Start apps:

```bash
pm2 start server.js
```

List apps:

```bash
pm2 list
```

Stop:

```bash
pm2 stop app-name
```

Restart:

```bash
pm2 restart app-name
```

Delete:

```bash
pm2 delete app-name
```

---

## 2. Auto Restart (Crash Recovery)

If Node crashes:

```
App crash ❌
```

PM2 automatically restarts it:

```
PM2 → restart app
```

Example reasons for crash:

* memory leak
* uncaught exception
* server overload

---

## 3. Cluster Mode (Multi CPU)

Node normally uses **one CPU core**.

PM2 can start multiple instances.

Example server:

```
CPU: 4 cores
```

Cluster mode:

```
Core1 → app1
Core2 → app2
Core3 → app3
Core4 → app4
```

Command:

```bash
pm2 start server.js -i max
```

`max` = number of CPU cores.

---

## 4. Load Balancing

PM2 distributes requests across instances.

Example:

```
Request
   ↓
PM2
 ↓ ↓ ↓ ↓
A1 A2 A3 A4
```

This improves **performance and scalability**.

---

## 5. Auto Start After Reboot

If the server restarts:

```
Server reboot
```

PM2 can automatically restore apps.

Setup:

```bash
pm2 startup
pm2 save
```

Now apps start automatically.

---

## 6. Monitoring

PM2 can show:

* CPU usage
* Memory usage
* Restarts
* uptime

Command:

```bash
pm2 monit
```

You will see a **live dashboard in terminal**.

---

## 7. Logs Management

Node logs can become messy.

PM2 manages logs.

View logs:

```bash
pm2 logs
```

Example output:

```
App logs
Error logs
```

Log files stored in:

```
~/.pm2/logs
```

---

# 4️⃣ Ecosystem Config (Production Setup)

Serious production apps use a config file.

Create:

```
ecosystem.config.js
```

Example:

```javascript
module.exports = {
  apps: [
    {
      name: "nextjs-app",
      script: ".next/standalone/server.js",
      instances: "max",
      exec_mode: "cluster",
      env: {
        NODE_ENV: "production"
      }
    }
  ]
}
```

Run:

```bash
pm2 start ecosystem.config.js
```

Advantages:

* clean configuration
* version controlled
* easy deployment

---

# 5️⃣ PM2 + Nginx Architecture

Typical production architecture:

```
Internet
   ↓
Nginx
   ↓
PM2
   ↓
Node.js Apps
```

Role of each component:

| Tool  | Role            |
| ----- | --------------- |
| Nginx | reverse proxy   |
| PM2   | process manager |
| Node  | application     |

Nginx handles:

* SSL
* caching
* static files
* load balancing

PM2 handles:

* process management
* clustering
* monitoring

---

# 6️⃣ PM2 Deployment Workflow

Typical workflow:

### 1. Build app

Example for Next.js:

```bash
npm run build
```

---

### 2. Start with PM2

```
pm2 start ecosystem.config.js
```

---

### 3. Save process list

```
pm2 save
```

---

### 4. Enable startup

```
pm2 startup
```

---

Now your server is **production-ready**.

---

# 7️⃣ Advantages of PM2

✔ keeps apps alive
✔ crash recovery
✔ clustering
✔ monitoring
✔ logs management
✔ zero downtime reload
✔ production ready

---

# 8️⃣ Disadvantages

⚠ not needed for small apps
⚠ memory overhead if many instances
⚠ cluster mode requires stateless apps

---

# 9️⃣ Alternatives

Other process managers:

* Forever (Node.js process manager)
* Supervisor (process control system)
* Docker
* systemd

But PM2 is the **most popular for Node.js**.

---

✅ **In summary**

PM2 is a **production process manager for Node.js** that provides:

* process management
* clustering
* auto restart
* monitoring
* log management

It is commonly used with:

* Next.js
* Express.js
* NestJS