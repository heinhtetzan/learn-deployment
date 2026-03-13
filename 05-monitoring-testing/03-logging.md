When debugging problems on an **Ubuntu VPS** (like your server running **Nginx + PM2 + Next.js / Node**), checking logs is the **first thing engineers do**. Logs tell you **what failed, when, and why**.

Below is a **practical guide used in real production servers**.

---

# 1️⃣ System Logs (Ubuntu)

Ubuntu stores system logs in:

```bash
/var/log
```

List logs:

```bash
ls /var/log
```

Common ones:

```
syslog
auth.log
kern.log
boot.log
```

---

## View system log

```bash
sudo less /var/log/syslog
```

Search for errors:

```bash
sudo grep -i error /var/log/syslog
```

---

## Watch log live

Very useful when debugging.

```bash
sudo tail -f /var/log/syslog
```

---

# 2️⃣ Authentication Logs (SSH Problems)

If SSH login fails or security issues happen:

```bash
/var/log/auth.log
```

Check login attempts:

```bash
sudo less /var/log/auth.log
```

Find failed login:

```bash
sudo grep "Failed password" /var/log/auth.log
```

Example output:

```
Failed password for root from 192.168.1.10
```

This helps detect **SSH attacks**.

---

# 3️⃣ Nginx Logs

Since you use **Nginx reverse proxy**, these logs are very important.

Location:

```bash
/var/log/nginx
```

List:

```bash
ls /var/log/nginx
```

Files:

```
access.log
error.log
```

---

## Nginx access log

Shows **all HTTP requests**.

```bash
sudo tail -f /var/log/nginx/access.log
```

Example:

```
GET /api/users 200
GET /login 404
```

---

## Nginx error log

Shows **server errors**.

```bash
sudo tail -f /var/log/nginx/error.log
```

Example:

```
connect() failed (111: Connection refused)
```

Common issue when Node app is not running.

---

# 4️⃣ PM2 Logs (Node / Next.js)

Since you run **Next.js with PM2**, this is extremely important.

Check logs:

```bash
pm2 logs
```

Specific app:

```bash
pm2 logs app-name
```

Example output:

```
Error: Cannot find module
```

---

## PM2 log location

```bash
~/.pm2/logs
```

List:

```bash
ls ~/.pm2/logs
```

Example:

```
next-app-out.log
next-app-error.log
```

Check error log:

```bash
less ~/.pm2/logs/next-app-error.log
```

Live monitoring:

```bash
tail -f ~/.pm2/logs/next-app-error.log
```

---

# 5️⃣ Systemd Service Logs

Ubuntu uses **systemd** to manage services.

Check logs of service:

Example for nginx:

```bash
journalctl -u nginx
```

Live logs:

```bash
journalctl -u nginx -f
```

---

Example for ssh:

```bash
journalctl -u ssh
```

---

# 6️⃣ Check Server Boot Errors

If server fails after reboot:

```bash
journalctl -b
```

Errors only:

```bash
journalctl -p err -b
```

---

# 7️⃣ Find Errors Fast (Important)

Search for errors across logs.

Example:

```bash
grep -i error /var/log/syslog
```

Search warnings:

```bash
grep -i warning /var/log/syslog
```

Search nginx errors:

```bash
grep -i error /var/log/nginx/error.log
```

---

# 8️⃣ Monitor Logs in Real Time (Production)

Very useful during deployment.

Example:

```bash
tail -f /var/log/nginx/error.log
```

Open another terminal:

```bash
pm2 restart app
```

Watch errors instantly.

---

# 9️⃣ Log Size Monitoring

Logs can grow very large.

Check size:

```bash
du -sh /var/log
```

List largest logs:

```bash
du -sh /var/log/*
```

Ubuntu usually uses **logrotate** to manage logs.

---

# 🔟 Quick Debug Workflow (Real Production)

Example when website fails.

Step 1 — check nginx

```bash
sudo systemctl status nginx
```

Step 2 — check nginx error

```bash
tail -f /var/log/nginx/error.log
```

Step 3 — check Node app

```bash
pm2 list
```

Step 4 — check Node logs

```bash
pm2 logs
```

Step 5 — check system logs

```bash
sudo tail -f /var/log/syslog
```

---

# 1️⃣1️⃣ Best Log Viewer Tool

Install **lnav** (very powerful).

```bash
sudo apt install lnav
```

Open logs:

```bash
lnav /var/log/syslog
```

Features:

* auto highlight errors
* search logs
* filter logs

Much easier than `less`.

---

# 1️⃣2️⃣ Most Important Logs For Your Stack

For your stack (**Ubuntu + Nginx + Next.js + PM2**):

Check these first:

```
/var/log/nginx/error.log
/var/log/nginx/access.log
~/.pm2/logs/
/var/log/syslog
/var/log/auth.log
```

---

✅ **Pro tip used by DevOps engineers**

Run this command when debugging server issues:

```bash
sudo tail -f /var/log/nginx/error.log ~/.pm2/logs/*.log
```

