Below is a **simple real-world example** of installing Docker and running **Laravel + Next.js** using Docker containers. This is a common setup for developers working with **full-stack apps** like you (Laravel API + Next.js frontend).

---

# 1️⃣ Install Docker (Ubuntu VPS)

```bash
sudo apt update

sudo apt install -y \
apt-transport-https \
ca-certificates \
curl \
gnupg \
lsb-release
```

Add Docker GPG key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Add Docker repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Install Docker

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

Enable Docker

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Test

```bash
docker --version
docker run hello-world
```

---

# 2️⃣ Project Structure

Example project:

```
my-app
│
├── docker-compose.yml
│
├── laravel
│   └── Dockerfile
│
└── nextjs
    └── Dockerfile
```

---

# 3️⃣ Laravel Dockerfile

`laravel/Dockerfile`

```dockerfile
FROM php:8.2-fpm

RUN apt update && apt install -y \
    git \
    curl \
    unzip \
    libpng-dev \
    libonig-dev \
    libxml2-dev

RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

COPY . .

RUN composer install

CMD ["php-fpm"]
```

---

# 4️⃣ Next.js Dockerfile

`nextjs/Dockerfile`

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm","start"]
```

---

# 5️⃣ Docker Compose (Laravel + Next + MySQL)

`docker-compose.yml`

```yaml
version: "3.9"

services:

  laravel:
    build: ./laravel
    container_name: laravel-app
    volumes:
      - ./laravel:/var/www
    ports:
      - "8000:8000"
    command: php artisan serve --host=0.0.0.0 --port=8000
    depends_on:
      - mysql

  nextjs:
    build: ./nextjs
    container_name: nextjs-app
    ports:
      - "3000:3000"

  mysql:
    image: mysql:8
    container_name: mysql-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: app
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```

---

# 6️⃣ Run the Application

Inside project root:

```bash
docker compose up -d
```

Check containers

```bash
docker ps
```

---

# 7️⃣ Access Applications

Laravel API

```
http://localhost:8000
```

Next.js Frontend

```
http://localhost:3000
```

MySQL

```
localhost:3306
user: root
password: secret
```

---

# 8️⃣ Useful Docker Commands

Start containers

```bash
docker compose up -d
```

Stop containers

```bash
docker compose down
```

Rebuild containers

```bash
docker compose up -d --build
```

Check logs

```bash
docker compose logs -f
```

Enter container

```bash
docker exec -it laravel-app bash
```

---

# 9️⃣ Real Production Architecture (Recommended)

For production you normally add:

```
Internet
   │
Nginx (reverse proxy)
   │
Docker Containers
   ├── Next.js
   ├── Laravel
   ├── Redis
   └── MySQL/PostgreSQL
```

Tools commonly used:

* **Docker Compose**
* **Nginx**
* **Redis**
* **PostgreSQL**
* **Certbot SSL**
* **CI/CD**