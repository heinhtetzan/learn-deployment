To run a **Next.js app on Ubuntu VPS with Nginx + PM2** using the domain **`next.kohtet.store`**, follow this simple production setup. 🚀

---

## 1️⃣ Install Required Packages

Update server first:

```bash
sudo apt update
sudo apt upgrade -y
```

Install Node.js (recommended with NVM):

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
```

Install Node LTS:

```bash
nvm install 20
nvm use 20
```

Install **PM2**:

```bash
npm install -g pm2
```

---

## 2️⃣ Upload / Clone Your Next.js Project

Example:

```bash
git clone https://github.com/your-repo/next-app.git
cd next-app
```

Install dependencies:

```bash
npm install
```

Build the project:

```bash
npm run build
```

---

## 3️⃣ Run Next.js with PM2

Run Next.js in production mode:

```bash
pm2 start npm --name "next-app" -- start
```

Check status:

```bash
pm2 list
```

Save PM2 process:

```bash
pm2 save
pm2 startup
```

Now your app runs on:

```
http://localhost:3000
```

---

## 4️⃣ Install Nginx

```bash
sudo apt install nginx -y
```

Start nginx:

```bash
sudo systemctl start nginx
```

---

## 5️⃣ Create Nginx Config

Create config:

```bash
sudo nano /etc/nginx/sites-available/next.kohtet.store
```

Add:

```nginx
server {
    listen 80;
    server_name next.kohtet.store;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;

        proxy_cache_bypass $http_upgrade;
    }
}
```

---

## 6️⃣ Enable the Site

```bash
sudo ln -s /etc/nginx/sites-available/next.kohtet.store /etc/nginx/sites-enabled/
```

Test config:

```bash
sudo nginx -t
```

Reload nginx:

```bash
sudo systemctl reload nginx
```

---

## 7️⃣ Point Domain DNS

In your domain DNS panel add:

```
Type: A
Name: next
Value: YOUR_VPS_IP
```

Example:

```
next.kohtet.store → 129.xxx.xxx.xxx
```

---

## 8️⃣ Add SSL (Recommended) 🔒

Install **Certbot**:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Run:

```bash
sudo certbot --nginx -d next.kohtet.store
```

Auto SSL renewal:

```bash
sudo certbot renew --dry-run
```

---

✅ Final result:

```
https://next.kohtet.store
```

Next.js → PM2 → Nginx → Domain