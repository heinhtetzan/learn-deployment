# 🚀 Full Stack Deployment Guide

Welcome to the collection of deployment guides and server management notes. This repository is designed to help you take your applications from local development to a secure, high-performance production environment on a VPS.

## 📌 How to use this guide
Follow these steps in order to set up your server and deploy your applications correctly.

---

### Phase 1: Server Preparation & Security
Before deploying any code, your server must be secured and updated.
1.  **[SSH Setup](file:///Users/kendrick/Desktop/doc/learn-deployment/01-setup-security/01-ssh-setup.md)**: Secure your access using SSH keys and disable root login.
2.  **[Server Security Checklist](file:///Users/kendrick/Desktop/doc/learn-deployment/01-setup-security/03-security-checklist.md)**: A complete list of vital security steps for a new VPS.
3.  **[Firewall (UFW)](file:///Users/kendrick/Desktop/doc/learn-deployment/01-setup-security/02-firewall-ufw.md)**: Control incoming and outgoing traffic.

---

### Phase 2: Database & Services Setup
Install the necessary components for your application's data layer.
-   **SQL Databases**: **[MySQL](file:///Users/kendrick/Desktop/doc/learn-deployment/02-databases/01-mysql.md)** or **[PostgreSQL](file:///Users/kendrick/Desktop/doc/learn-deployment/02-databases/02-postgresql.md)**.
-   **Key-Value Stores**: **[Redis](file:///Users/kendrick/Desktop/doc/learn-deployment/02-databases/03-redis.md)** for caching and sessions.
-   **Containers**: **[Docker](file:///Users/kendrick/Desktop/doc/learn-deployment/06-devops/01-docker.md)** if you prefer containerized deployments.

---

### Phase 3: Application Deployment
Detailed guides for specific frameworks.
-   🐘 **[Laravel VPS Deployment](file:///Users/kendrick/Desktop/doc/learn-deployment/03-applications/01-laravel-vps.md)**: Full setup with PHP 8.4, Nginx, and SQLite/MySQL.
-   ⚛️ **[Next.js VPS Deployment](file:///Users/kendrick/Desktop/doc/learn-deployment/03-applications/02-nextjs-vps.md)**: Deploy using Nginx as a reverse proxy and PM2 for process management.
-   🏗️ **[Stateless Backend Practices](file:///Users/kendrick/Desktop/doc/learn-deployment/03-applications/03-stateless-practices.md)**: Learn how to build apps that scale.
-   ✨ **[Coolify (Self-Hosted PaaS)](file:///Users/kendrick/Desktop/doc/learn-deployment/06-devops/02-coolify.md)**: A modern alternative to Heroku/Vercel for easy deployments.

---

### Phase 4: Process Management & Monitoring
Ensure your applications stay online and performant.
-   **Process Managers**: Use **[PM2](file:///Users/kendrick/Desktop/doc/learn-deployment/04-process-management/01-pm2.md)** for Node.js or **[Supervisor](file:///Users/kendrick/Desktop/doc/learn-deployment/04-process-management/02-supervisor.md)** for PHP/Laravel queues.
-   **Monitoring**: **[Monitoring Commands](file:///Users/kendrick/Desktop/doc/learn-deployment/05-monitoring-testing/01-monitoring-commands.md)** to check server health in real-time.
-   **Performance**: Run **[Performance Tests](file:///Users/kendrick/Desktop/doc/learn-deployment/05-monitoring-testing/02-performance-testing.md)** to benchmark your setup.
-   **Logs**: Manage your application and system logs with **[Logging Best Practices](file:///Users/kendrick/Desktop/doc/learn-deployment/05-monitoring-testing/03-logging.md)**.

---

## 🛠 Useful Utilities
-   [01-laravel-vps.md](file:///Users/kendrick/Desktop/doc/learn-deployment/03-applications/01-laravel-vps.md) - Laravel specific optimization.
-   [02-nextjs-vps.md](file:///Users/kendrick/Desktop/doc/learn-deployment/03-applications/02-nextjs-vps.md) - Next.js production build steps.

Happy Deploying! 🚀
