A **firewall** controls **which network traffic is allowed or blocked** on your server.
For an Ubuntu VPS (like the one you run for **Nginx + Next.js + PM2**), the most common firewall is **UFW (Uncomplicated Firewall)**.

It sits between **internet traffic and your server** and decides:

```
Internet → Firewall → Server
```

Example:

```
Allow: 22 (SSH)
Allow: 80 (HTTP)
Allow: 443 (HTTPS)
Block: everything else
```

This greatly improves server security. 🔐

---

# 1️⃣ Check Firewall Status

Ubuntu firewall command:

```bash
sudo ufw status
```

Example output:

```
Status: active

22/tcp   ALLOW
80/tcp   ALLOW
443/tcp  ALLOW
```

Meaning:

* SSH allowed
* Web traffic allowed
* Everything else blocked

---

# 2️⃣ Install Firewall (if not installed)

Most Ubuntu servers already have UFW.

Install if needed:

```bash
sudo apt install ufw
```

---

# 3️⃣ Enable Firewall

Before enabling, allow SSH or you will lock yourself out.

```bash
sudo ufw allow ssh
```

Then enable:

```bash
sudo ufw enable
```

Check status again:

```bash
sudo ufw status
```

---

# 4️⃣ Allow Important Ports

Typical web server ports:

### SSH

```bash
sudo ufw allow 22
```

or

```bash
sudo ufw allow ssh
```

---

### HTTP

```bash
sudo ufw allow 80
```

---

### HTTPS

```bash
sudo ufw allow 443
```

---

### Custom App Port (example Node)

If your Node app runs on **3000**:

```bash
sudo ufw allow 3000
```

But normally with **Nginx reverse proxy**, you **should NOT expose 3000 publicly**.

Better setup:

```
Internet → 80/443 → Nginx → 3000 Node
```

So only allow:

```
22
80
443
```

---

# 5️⃣ Deny Port

Block a port:

```bash
sudo ufw deny 3000
```

---

# 6️⃣ Remove Rule

List numbered rules:

```bash
sudo ufw status numbered
```

Example:

```
[1] 22/tcp ALLOW
[2] 80/tcp ALLOW
```

Delete rule:

```bash
sudo ufw delete 2
```

---

# 7️⃣ Allow Specific IP Only

Example: allow SSH only from your IP.

```bash
sudo ufw allow from 1.2.3.4 to any port 22
```

This is **very secure for production**.

---

# 8️⃣ Allow Port + Protocol

Example:

```bash
sudo ufw allow 80/tcp
```

or

```bash
sudo ufw allow 53/udp
```

---

# 9️⃣ Check Firewall Logs

Logs help detect attacks.

Enable logging:

```bash
sudo ufw logging on
```

Check logs:

```bash
sudo less /var/log/ufw.log
```

---

# 🔟 Reset Firewall

If configuration breaks:

```bash
sudo ufw reset
```

Then configure again.

---

# 1️⃣1️⃣ Recommended Production Rules

For your stack (**Ubuntu + Nginx + PM2 + Next.js**):

```
22  SSH
80  HTTP
443 HTTPS
```

Commands:

```bash
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

Check:

```bash
sudo ufw status
```

---

# 1️⃣2️⃣ Check Open Ports vs Firewall

Check running ports:

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

Firewall may allow only:

```
22
80
443
```

So **port 3000 remains private**.

---

# 1️⃣3️⃣ Best Firewall Practice

A production server should follow:

```
Default: deny incoming
Allow only necessary ports
```

Check default rules:

```bash
sudo ufw status verbose
```

Typical output:

```
Default: deny (incoming)
Default: allow (outgoing)
```

---

# 1️⃣4️⃣ Firewall + Fail2Ban (Powerful Security)

Firewall blocks ports.
Fail2ban blocks **attackers automatically**.

Example:

```
Attacker tries SSH password 20 times
→ Fail2Ban bans IP
→ Firewall blocks IP
```

Install:

```bash
sudo apt install fail2ban
```

---

# 1️⃣5️⃣ Real VPS Security Setup (Typical)

After creating a server:

```
1. Setup SSH key login
2. Disable root password login
3. Enable firewall
4. Allow ports 22 80 443
5. Install fail2ban
6. Setup Nginx reverse proxy
```

---

✅ For your **Next.js VPS**, the **ideal firewall configuration** is:

```bash
sudo ufw allow 22
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

Everything else should remain blocked.