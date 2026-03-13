## What is Supervisor?

**Supervisor (process control system)** is a **process manager for Linux/Unix systems** that keeps programs running continuously.

It is written in **Python** and commonly used on servers like **Ubuntu VPS**.

A **process manager** ensures that applications:

* start automatically
* restart if they crash
* run in the background
* start again after server reboot
* log their output

Supervisor works with **any language**, not just one.

Examples:

* Node.js
* PHP workers
* Go servers
* Python apps
* background scripts

---

# 1️⃣ Why Supervisor Exists

Normally when you run an application:

```bash
node server.js
```

or

```bash
php worker.php
```

the app runs inside your terminal.

Problem:

```
Terminal closed → app stops
Server reboot → app stops
App crash → app stops
```

This is **not acceptable for production servers**.

Supervisor solves this problem.

Architecture:

```
Server
  ↓
supervisord (daemon)
  ↓
Managed Programs
```

`supervisord` runs in the background and manages all apps.

---

# 2️⃣ Core Components

Supervisor has two main parts.

### supervisord

The **background service**.

It manages processes.

```
supervisord
   ↓
program1
program2
program3
```

---

### supervisorctl

The **command tool** used to control processes.

Examples:

```bash
sudo supervisorctl status
sudo supervisorctl restart app
sudo supervisorctl stop app
```

---

# 3️⃣ Installing Supervisor (Ubuntu)

Typical installation:

```bash
sudo apt update
sudo apt install supervisor
```

Start service:

```bash
sudo systemctl start supervisor
```

Enable auto start:

```bash
sudo systemctl enable supervisor
```

Config directory:

```
/etc/supervisor/
```

Program configs usually go in:

```
/etc/supervisor/conf.d/
```

---

# 4️⃣ Basic Supervisor Configuration

Example file:

```
/etc/supervisor/conf.d/myapp.conf
```

Example:

```ini
[program:myapp]
command=node server.js
directory=/var/www/app
autostart=true
autorestart=true
stderr_logfile=/var/log/myapp.err.log
stdout_logfile=/var/log/myapp.out.log
```

Explanation:

| Option         | Meaning                      |
| -------------- | ---------------------------- |
| program        | name of program              |
| command        | command to run               |
| directory      | working directory            |
| autostart      | start when supervisor starts |
| autorestart    | restart if crash             |
| stdout_logfile | output log                   |
| stderr_logfile | error log                    |

Reload supervisor:

```bash
sudo supervisorctl reread
sudo supervisorctl update
```

---

# 5️⃣ Using Supervisor with Node.js

Example Node server:

```javascript
const http = require("http");

http.createServer((req,res)=>{
  res.end("Hello from Node");
}).listen(3000);
```

Supervisor config:

```ini
[program:nodeapp]
command=node server.js
directory=/var/www/nodeapp
autostart=true
autorestart=true
stdout_logfile=/var/log/nodeapp.log
```

Start:

```bash
sudo supervisorctl start nodeapp
```

Now:

```
Node app crash → Supervisor restarts it
```

---

# 6️⃣ Using Supervisor with Next.js

For **Next.js** production server:

Build project first:

```bash
npm run build
npm start
```

Supervisor config:

```ini
[program:nextjs]
command=npm start
directory=/var/www/nextjs-app
autostart=true
autorestart=true
stdout_logfile=/var/log/nextjs.log
```

Architecture:

```
Internet
   ↓
Nginx
   ↓
Next.js (Supervisor managed)
```

Supervisor ensures Next.js **always stays running**.

---

# 7️⃣ Using Supervisor with PHP

PHP web apps usually run under **Apache HTTP Server** or **Nginx**, so Supervisor is not used for the web server itself.

Instead it is used for **background workers**.

Example: **Laravel queue worker**

Command:

```bash
php artisan queue:work
```

Supervisor config:

```ini
[program:laravel-worker]
command=php artisan queue:work
directory=/var/www/laravel
autostart=true
autorestart=true
numprocs=3
stdout_logfile=/var/log/laravel-worker.log
```

`numprocs=3` means **three workers** run simultaneously.

---

# 8️⃣ Using Supervisor with Go

Go applications compile into **binary executables**.

Example Go server compiled as:

```
app
```

Run normally:

```bash
./app
```

Supervisor config:

```ini
[program:goapp]
command=/var/www/goapp/app
directory=/var/www/goapp
autostart=true
autorestart=true
stdout_logfile=/var/log/goapp.log
```

Supervisor ensures the Go service **never stops**.

---

# 9️⃣ Monitoring Programs

Check status:

```bash
sudo supervisorctl status
```

Example output:

```
nodeapp       RUNNING
nextjs        RUNNING
laravel-worker RUNNING
goapp         RUNNING
```

Restart program:

```bash
sudo supervisorctl restart nodeapp
```

Stop:

```bash
sudo supervisorctl stop nodeapp
```

---

# 🔟 Typical Production Architecture

Example full stack:

```
Internet
   ↓
Nginx
   ↓
Application Server
   ↓
Supervisor
   ↓
Apps
```

Example apps managed:

```
Node API
Next.js frontend
Laravel queue workers
Go microservice
```

Supervisor manages **all background processes**.

---

# Advantages of Supervisor

✔ language independent
✔ very stable
✔ simple configuration
✔ good for background workers
✔ widely used in Linux servers

---

# Limitations

❌ no cluster load balancing
❌ no advanced monitoring dashboard
❌ fewer Node-specific features

This is why many Node projects prefer **PM2**.

---

✅ **Summary**

Supervisor is a **general Linux process manager** that keeps programs running continuously.

It works with:

* Node.js servers
* Next.js production servers
* PHP background workers
* Go binaries

Its main job is **process management and automatic recovery** for server applications.