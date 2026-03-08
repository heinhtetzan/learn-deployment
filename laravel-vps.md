
# Deploy Laravel Starter-kit on VPS

✅ Tech Stack Summary
* **Git repo:** `https://github.com/heinhtetzan/laravel-react-starter-kit`
* **Domain:** `laravel-react-starter-kit.teapos.shop`

| Component   | Version                 |
| ----------- | ----------------------- |
| OS          | Ubuntu 22.04            |
| Web Server  | Nginx                   |
| PHP         | **8.4**                 |
| PHP Handler | PHP-FPM 8.4             |
| Backend     | Laravel framework       |
| Frontend    | React (Vite)            |
| Database    | SQLite                  |
| SSL         | Let’s Encrypt (Certbot) |

---

## 1️⃣ Update server & basic setup

```bash
sudo apt update && sudo apt upgrade -y
sudo timedatectl set-timezone Asia/Bangkok
```

Install basic tools:

```bash
sudo apt install -y curl unzip git software-properties-common
```

---

## 2️⃣ Install Nginx

```bash
sudo apt install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

Firewall (recommended):

```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw enable
```

---

## 3️⃣ Install PHP 8.4 & PHP-FPM

Add Ondřej PHP repository:

```bash
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
```

Install **PHP 8.4** with required extensions:

```bash
sudo apt install -y \
php8.4-fpm php8.4-cli php8.4-common \
php8.4-mbstring php8.4-xml php8.4-curl \
php8.4-zip php8.4-sqlite3 php8.4-bcmath
```

Verify:

```bash
php -v
```

Enable PHP-FPM:

```bash
sudo systemctl enable php8.4-fpm
sudo systemctl start php8.4-fpm
```

Set PHP 8.4 as default:

```bash
sudo update-alternatives --set php /usr/bin/php8.4
php -v
```

---

## 4️⃣ Install Composer (PHP 8.4 compatible)

```bash
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
composer --version
```

---

## 5️⃣ Deploy the Laravel + React Starter Kit

Clone your repo:

```bash
cd /var/www
sudo git clone https://github.com/heinhtetzan/laravel-react-starter-kit.git
```

Rename (optional but clean):

```bash
sudo mv laravel-react-starter-kit app
```

Set permissions:

```bash
sudo chown -R www-data:www-data /var/www/app
sudo chmod -R 775 /var/www/app/storage /var/www/app/bootstrap/cache
```

---

## 6️⃣ Install backend dependencies (Laravel)

```bash
cd /var/www/app
composer install --no-dev --optimize-autoloader
```

---

## 7️⃣ Configure SQLite

Create database file:

```bash
touch database/database.sqlite
```

Set permissions:

```bash
sudo chown www-data:www-data database/database.sqlite
sudo chmod 664 database/database.sqlite
```


Edit `.env`:

```bash
cp .env.example .env
nano .env
```

Update:

```env
APP_NAME=LaravelReactStarter
APP_ENV=production
APP_DEBUG=false
APP_URL=https://laravel-react-starter-kit.teapos.shop

DB_CONNECTION=sqlite
DB_DATABASE=/var/www/app/database/database.sqlite
```

Generate key & migrate:

```bash
php artisan key:generate
php artisan migrate --force
```

---

## 8️⃣ Build React frontend

(Assuming Vite is used)

Install Node.js (LTS):

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
```

Install frontend dependencies & build:

```bash
npm install
npm run build
```

> This will generate files in `public/build`

---

## 9️⃣ Configure Nginx (PHP 8.4)

Create Nginx config:

```bash
sudo nano /etc/nginx/sites-available/laravel-react-starter-kit.teapos.shop
```

Paste:

```nginx
server {
    listen 80;
    server_name laravel-react-starter-kit.teapos.shop;

    root /var/www/app/public;
    index index.php index.html;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/laravel-react-starter-kit.teapos.shop /etc/nginx/sites-enabled/

sudo rm /etc/nginx/sites-enabled/default
```

Test & reload:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 🔟 Laravel production optimization

```bash
php artisan optimize
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan storage:link
```

---

## 1️⃣ 1️⃣ Install SSL (Certbot)

```bash
sudo apt install -y certbot python3-certbot-nginx
```

Generate SSL:

```bash
sudo certbot --nginx -d laravel-react-starter-kit.teapos.shop
```

Enable auto-renew:

```bash
sudo systemctl enable certbot.timer
sudo certbot renew --dry-run
```

---

## 1️⃣ 2️⃣ Final checks

```bash
sudo systemctl status nginx
sudo systemctl status php8.4-fpm
```

Visit:

```
https://laravel-react-starter-kit.teapos.shop
```

---

## 🔧 Useful Debug Commands

```bash
php -v
php-fpm8.4 -t
sudo tail -f /var/log/nginx/error.log
sudo tail -f /var/log/php8.4-fpm.log
```
