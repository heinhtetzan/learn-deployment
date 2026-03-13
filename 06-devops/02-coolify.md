# ✨ Deploy with Coolify (Self-Hosted PaaS)

[Coolify](https://coolify.io/) is an open-source & self-hosted alternative to Heroku, Netlify, and Vercel. It allows you to manage your own servers and deploy applications with ease via a beautiful dashboard.

---

## 1️⃣ Prerequisites
- A clean **Ubuntu 22.04+** VPS.
- At least **2GB RAM** (4GB recommended).
- A domain name (optional but recommended).

---

## 2️⃣ One-Line Installation
Log in to your server via SSH and run the official installation script:

```bash
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

This will automatically:
1. Install Docker and required dependencies.
2. Pull the Coolify Docker images.
3. Start the Coolify dashboard on port 8000.

---

## 3️⃣ Initial Setup
1. Visit `http://your-vps-ip:8000` in your browser.
2. Complete the initial configuration (Admin email and password).
3. Connect your first **Server** (usually the "localhost" you just installed it on).

---

## 4️⃣ Deploying an Application
Coolify makes it extremely easy to deploy frameworks like Laravel, Next.js, and more:
1.  **Sources**: Connect your GitHub, GitLab, or Bitbucket account.
2.  **Resources**: Click "Create New Resource" and choose "Public Repository" or "Private Repository".
3.  **Automatic Detection**: Coolify will detect your project type (Docker, Nixpacks, or Static).
4.  **Deployment**: Click "Deploy" and watch the logs!

---

## 🚀 Key Features
- **Auto-SSL**: Automatically generates Let's Encrypt certificates for your domains.
- **Database Management**: Spin up MySQL, PostgreSQL, Redis, or MongoDB with one click.
- **Webhooks**: Automatic deployments when you push to GitHub.
- **Monitoring**: Built-in health checks and deployment logs.

---

## 💡 Troubleshooting
If you can't access the dashboard, ensure your firewall allows port 8000:
```bash
sudo ufw allow 8000
```
