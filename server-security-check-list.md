When you run a **public VPS (Ubuntu) with Nginx + PM2 + Next.js**, security is extremely important. A production server should always be checked for:

1️⃣ **Access security (SSH)**
2️⃣ **Network security (ports / firewall)**
3️⃣ **System updates**
4️⃣ **Intrusion attempts**
5️⃣ **File permissions**
6️⃣ **Running services**

Below is a **practical server security checklist with commands**.

---

# 1️⃣ Check Logged-in Users

See who is currently connected to the server.

```bash
who
```

or

```bash
w
```

Example output:

```
root pts/0 192.168.1.10
```

Shows:

* user
* login time
* IP address

---

# 2️⃣ Check Login History

Check previous logins.

```bash
last
```

Example:

```
root  192.168.1.20  Mon Mar 9
```

Check failed login attempts:

```bash
sudo grep "Failed password" /var/log/auth.log
```

If many failures appear → **possible brute force attack**.

---

# 3️⃣ Check Open Ports

See which ports are accessible.

```bash
ss -tulpn
```

Example:

```
22   ssh
80   nginx
443  nginx
3000 node
```

In a normal web server, usually only:

```
22  (SSH)
80  (HTTP)
443 (HTTPS)
```

should be public.

---

# 4️⃣ Check Firewall Status

Ubuntu firewall:

```bash
sudo ufw status
```

Example:

```
22/tcp   ALLOW
80/tcp   ALLOW
443/tcp  ALLOW
```

Enable firewall (if not enabled):

```bash
sudo ufw enable
```

---

# 5️⃣ Check Running Services

List services:

```bash
systemctl list-units --type=service
```

Check specific service:

```bash
sudo systemctl status nginx
```

Look for **unknown services**.

---

# 6️⃣ Check Suspicious Processes

See running processes:

```bash
ps aux
```

Or use better tool:

```bash
htop
```

Look for:

* unknown programs
* high CPU usage
* crypto miners

---

# 7️⃣ Check Installed Packages

List installed software:

```bash
dpkg -l
```

Look for suspicious packages.

---

# 8️⃣ Check System Updates

Outdated systems are vulnerable.

Check updates:

```bash
sudo apt update
```

Upgrade:

```bash
sudo apt upgrade
```

---

# 9️⃣ Check Disk for Unknown Files

Check large files:

```bash
sudo find / -type f -size +200M
```

Check suspicious hidden files:

```bash
ls -la
```

Look for strange files like:

```
.x
.tmp
.hidden_script
```

---

# 🔟 Check Root Access

See who can use root:

```bash
cat /etc/passwd
```

Root users should be minimal.

Check sudo users:

```bash
getent group sudo
```

---

# 1️⃣1️⃣ Check SSH Security

Open SSH config:

```bash
sudo nano /etc/ssh/sshd_config
```

Important settings:

```
PermitRootLogin no
PasswordAuthentication no
```

This prevents password attacks.

Restart SSH:

```bash
sudo systemctl restart ssh
```

---

# 1️⃣2️⃣ Check File Permissions

Private keys must be protected.

Example:

```bash
ls -l ~/.ssh
```

Secure permissions:

```
700 ~/.ssh
600 authorized_keys
```

Fix:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

# 1️⃣3️⃣ Check Cron Jobs (Scheduled Tasks)

Attackers sometimes add hidden cron jobs.

Check:

```bash
crontab -l
```

System cron:

```bash
ls /etc/cron.*
```

---

# 1️⃣4️⃣ Check Network Connections

See active connections.

```bash
netstat -antp
```

or

```bash
ss -antp
```

Look for unknown IP addresses.

---

# 1️⃣5️⃣ Check Log for Attacks

SSH attack check:

```bash
sudo grep "Failed password" /var/log/auth.log
```

Nginx error:

```bash
sudo tail -f /var/log/nginx/error.log
```

---

# 1️⃣6️⃣ Install Security Tools (Recommended)

### Fail2Ban

Blocks brute force attacks.

Install:

```bash
sudo apt install fail2ban
```

Check status:

```bash
sudo systemctl status fail2ban
```

---

### ClamAV (virus scanner)

```bash
sudo apt install clamav
```

Scan system:

```bash
sudo clamscan -r /
```

---

# 1️⃣7️⃣ Check Rootkits

Install rootkit detector:

```bash
sudo apt install rkhunter
```

Scan:

```bash
sudo rkhunter --check
```

---

# 1️⃣8️⃣ Automatic Security Scan Tool

Install **Lynis** (very powerful).

```bash
sudo apt install lynis
```

Run scan:

```bash
sudo lynis audit system
```

It will show:

* vulnerabilities
* misconfigurations
* security score

---

# 1️⃣9️⃣ Production Security Setup (Recommended)

A secure VPS should have:

```
SSH key login only
Firewall enabled
Fail2ban installed
Regular updates
Limited open ports
```

Typical open ports:

```
22
80
443
```

---

# 2️⃣0️⃣ Quick Security Audit (Engineer Routine)

When logging into a server:

```bash
ssh server
```

Run quick checks:

```bash
who
last
ss -tulpn
sudo ufw status
htop
```

Check logs:

```bash
sudo tail -f /var/log/auth.log
```

---

✅ For someone like you running **VPS with Nginx + Node + PM2**, the **most important protections are**:

1️⃣ SSH key login
2️⃣ Firewall
3️⃣ Fail2ban
4️⃣ Regular updates
5️⃣ Log monitoring
