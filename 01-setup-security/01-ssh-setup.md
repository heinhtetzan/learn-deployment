SSH is one of the **most important tools for developers managing servers** (like your VPS running Nginx / Next.js / Laravel). I’ll explain it in a **clear production-oriented way**.

---

# 1. What is SSH?

**SSH (Secure Shell)** is a protocol that allows you to **securely connect to a remote server through a terminal**.

Example:

```
Your Laptop  →  Internet  →  VPS Server
        SSH encrypted connection
```

Typical use cases:

• Manage VPS
• Deploy applications
• Transfer files
• Run commands remotely
• Manage Docker / PM2 / Nginx / databases

Example:

```bash
ssh root@129.212.233.78
```

You now control the **remote Linux machine**.

---

# 2. Why SSH is Important

Without SSH, managing servers would require **physical access**.

With SSH you can:

• Deploy **Next.js / Node / Laravel apps**
• Restart services
• Configure **Nginx / PM2 / MySQL**
• Upload files
• Monitor logs

Example workflow (your case):

```
MacBook → SSH → Ubuntu VPS
                  ↓
              PM2 start
              Nginx config
              Certbot SSL
```

SSH is basically the **standard DevOps remote access protocol**.

---

# 3. How SSH Works (Simplified)

SSH works using **encryption + authentication**.

Steps:

1️⃣ Client connects

```
ssh root@server_ip
```

2️⃣ Server verifies identity

3️⃣ Authentication happens

Two types:

### Password Authentication

```
ssh root@129.212.233.78
Password: ********
```

Not recommended for production.

---

### SSH Key Authentication (Best Practice)

Uses **public / private key cryptography**.

```
Your Laptop
   private key  → authenticate
Server
   public key   → verify
```

Example:

```
~/.ssh/id_rsa        (private key)
~/.ssh/id_rsa.pub    (public key)
```

---

# 4. Generate SSH Key

On your MacBook:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Result:

```
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Private key:

```
id_ed25519
```

Public key:

```
id_ed25519.pub
```

---

# 5. Connect to VPS Using Key

Upload public key to server.

Example:

```bash
ssh-copy-id root@129.212.233.78
```

Or manually:

```
~/.ssh/authorized_keys
```

Server file:

```
/root/.ssh/authorized_keys
```

Now connect:

```bash
ssh root@129.212.233.78
```

No password required.

---

# 6. Important SSH Commands

### Connect to server

```bash
ssh user@server_ip
```

---

### Connect using specific key

```bash
ssh -i ~/.ssh/mykey root@129.212.233.78
```

---

### Copy file to server

```bash
scp file.txt root@server:/var/www/
```

---

### Copy folder

```bash
scp -r project root@server:/var/www/
```

---

### Execute command remotely

```bash
ssh root@server "pm2 restart app"
```

---

### Exit SSH

```
exit
```

or

```
CTRL + D
```

---

# 7. Managing Multiple VPS

When you have many servers:

Example:

```
Production VPS
Staging VPS
Database VPS
Backup VPS
```

Instead of remembering IPs, use **SSH config**.

File:

```
~/.ssh/config
```

Example:

```bash
Host prod
  HostName 129.212.233.78
  User root
  IdentityFile ~/.ssh/prod_key

Host staging
  HostName 167.99.22.11
  User root
  IdentityFile ~/.ssh/staging_key
```

Now connect easily:

```
ssh prod
```

or

```
ssh staging
```

This is **very common in DevOps teams**.

---

# 8. SSH Security Best Practices (VERY IMPORTANT)

Production servers should follow these.

### 1️⃣ Disable password login

Edit:

```
/etc/ssh/sshd_config
```

Change:

```
PasswordAuthentication no
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

---

### 2️⃣ Disable root login

```
PermitRootLogin no
```

Use a normal user instead.

---

### 3️⃣ Change SSH port

Default:

```
22
```

Change to:

```
Port 2222
```

Then connect:

```
ssh -p 2222 user@server
```

---

### 4️⃣ Use firewall

Example with UFW:

```bash
sudo ufw allow 22
sudo ufw enable
```

---

### 5️⃣ Install fail2ban

Protects from brute force attacks.

```
sudo apt install fail2ban
```

---

# 9. SSH Key Management (Very Important for Device Change)

If you change laptop or device.

Example situation:

```
Old MacBook
New MacBook
```

You need SSH access.

---

### Best practice approach

Generate **new key per device**.

Example:

```
MacBook
→ macbook_key

Desktop
→ desktop_key

Work laptop
→ work_key
```

Each key added to server:

```
~/.ssh/authorized_keys
```

Example:

```
macbook_key.pub
desktop_key.pub
```

---

### If device is lost

Simply remove its key.

Edit server:

```
~/.ssh/authorized_keys
```

Delete that key.

Access immediately revoked.

---

# 10. SSH Agent (Very Useful)

SSH agent manages keys in memory.

Start agent:

```bash
eval "$(ssh-agent -s)"
```

Add key:

```bash
ssh-add ~/.ssh/id_ed25519
```

Now SSH works without typing key path.

---

# 11. Production Deployment Flow (Your Case)

Typical real workflow:

```
MacBook
   ↓
SSH
   ↓
Ubuntu VPS
   ↓
git pull
npm install
npm run build
pm2 restart app
```

Example:

```bash
ssh prod
cd /var/www/next-app
git pull
npm run build
pm2 restart ecosystem.config.js
```

---

# 12. Tools That Use SSH

Many dev tools internally use SSH.

Examples:

• GitHub
• GitLab
• Docker remote
• Ansible
• VSCode Remote SSH
• rsync
• scp

Example GitHub:

```
git@github.com:user/repo.git
```

Uses SSH.

---

# 13. Common SSH Problems

### Permission too open

Example error:

```
WARNING: UNPROTECTED PRIVATE KEY FILE
```

Fix:

```bash
chmod 600 ~/.ssh/id_ed25519
```

---

### Cannot connect

Check:

```
ping server_ip
```

Check port:

```
nc -zv server_ip 22
```

---

# 14. Real DevOps SSH Structure (Recommended)

Professional setup:

```
~/.ssh

id_ed25519
id_ed25519.pub

config

prod_vps_key
staging_vps_key
github_key
```

SSH config:

```
ssh prod
ssh staging
ssh github
```

Clean and scalable.

---

✅ Since you're managing **VPS + Nginx + PM2 + Next.js**, SSH will be your **main DevOps gateway**.