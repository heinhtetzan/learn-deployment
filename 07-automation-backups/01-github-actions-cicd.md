# 🤖 CI/CD with GitHub Actions

Automate your deployments so that every time you push code to GitHub, it automatically updates on your VPS.

## 1️⃣ Prerequisites
- A GitHub repository for your project.
- SSH access to your VPS.
- An SSH Private Key (stored as a GitHub Secret).

---

## 2️⃣ Setting up GitHub Secrets
In your GitHub repo, go to **Settings > Secrets and variables > Actions** and add:
- `SSH_PRIVATE_KEY`: Your private key (typically `~/.ssh/id_rsa`).
- `REMOTE_HOST`: Your VPS IP address.
- `REMOTE_USER`: Your SSH username (e.g., `root` or `ubuntu`).

---

## 3️⃣ Example Workflow (Laravel)
Create a file at `.github/workflows/deploy.yml`:

```yaml
name: Deploy to VPS

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy via SSH
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.REMOTE_HOST }}
          username: ${{ secrets.REMOTE_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /var/www/app
            git pull origin main
            composer install --no-dev --optimize-autoloader
            php artisan migrate --force
            php artisan optimize
            sudo systemctl reload php8.4-fpm
```

---

## 4️⃣ Example Workflow (Next.js)
```yaml
      - name: Deploy Next.js
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.REMOTE_HOST }}
          username: ${{ secrets.REMOTE_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /var/www/next-app
            git pull origin main
            npm install
            npm run build
            pm2 restart next-app
```

---

## 💡 Pro Tip
Use **Zero-Downtime Deployment** tools like `Deployer` (PHP) or blue-green strategies for more complex apps.
