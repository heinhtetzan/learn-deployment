When managing an **Ubuntu VPS** (like the one you’re using for **Nginx + PM2 + Next.js**), there are three main areas you should monitor:

1️⃣ **Server resources** (CPU, RAM, processes)
2️⃣ **Storage** (disk usage, I/O)
3️⃣ **File management** (finding, inspecting, editing files)

Below are the **most important production commands** every DevOps / backend engineer uses.

---

# 1️⃣ Resource Monitoring (CPU / RAM / Processes)

### Check system overview

```bash
top
```

Shows:

* CPU usage
* RAM usage
* running processes
* load average

Example output:

```
CPU: 12% user
Mem: 2GB / 8GB
```

Press **q** to exit.

---

### Better version of top

Install:

```bash
sudo apt install htop
```

Run:

```bash
htop
```

Advantages:

* colored UI
* easier process management
* kill process interactively

Example use:

* detect high CPU Node process
* detect memory leak

---

### Check CPU usage

```bash
lscpu
```

Shows:

* CPU cores
* architecture
* virtualization

Example:

```
CPU(s): 4
Model name: Intel Xeon
```

---

### Check RAM usage

```bash
free -h
```

Example:

```
              total   used   free
Mem:           8G     2.1G    5.9G
```

`-h` = human readable.

---

### Check server uptime

```bash
uptime
```

Example:

```
10:23 up 3 days, load average: 0.30 0.40 0.50
```

Load average shows CPU pressure.

---

### Show running processes

```bash
ps aux
```

Example filter Node:

```bash
ps aux | grep node
```

---

### Kill process

Find PID:

```bash
ps aux | grep node
```

Kill:

```bash
kill PID
```

Force kill:

```bash
kill -9 PID
```

---

# 2️⃣ Storage Monitoring (Disk / I/O)

### Check disk usage

```bash
df -h
```

Example:

```
Filesystem      Size  Used Avail Use%
/dev/vda1        50G   12G   38G  24%
```

Very important for production.

---

### Check folder size

```bash
du -sh *
```

Example output:

```
2G logs
500M uploads
```

Great for finding large directories.

---

### Disk usage of specific folder

```bash
du -sh /var/www
```

---

### Find large files

```bash
find / -type f -size +100M
```

Find files larger than **100MB**.

---

### Check disk I/O

Install:

```bash
sudo apt install iotop
```

Run:

```bash
sudo iotop
```

Shows which process is using disk.

---

# 3️⃣ Network Monitoring

### Check open ports

```bash
ss -tulpn
```

Example output:

```
:80   nginx
:443  nginx
:3000 node
:22   ssh
```

Useful when debugging **Nginx + Next.js**.

---

### Check internet traffic

Install:

```bash
sudo apt install nload
```

Run:

```bash
nload
```

Shows:

* upload speed
* download speed

---

### Check server IP

```bash
ip a
```

---

### Test connection

```bash
ping google.com
```

---

# 4️⃣ File Management Commands

These are **daily Linux commands**.

---

### List files

```bash
ls
```

Detailed:

```bash
ls -la
```

Example:

```
drwxr-xr-x project
-rw-r--r-- config.js
```

---

### Change directory

```bash
cd folder
```

Example:

```bash
cd /var/www
```

---

### Show current directory

```bash
pwd
```

---

### Create folder

```bash
mkdir project
```

---

### Remove file

```bash
rm file.txt
```

Remove folder:

```bash
rm -r folder
```

Force remove:

```bash
rm -rf folder
```

Be careful ⚠️

---

### Copy files

```bash
cp file.txt backup.txt
```

Copy folder:

```bash
cp -r project backup
```

---

### Move / rename

```bash
mv old.txt new.txt
```

Move folder:

```bash
mv project /var/www
```

---

### Edit files

Using nano:

```bash
nano nginx.conf
```

Save:

```
CTRL + O
```

Exit:

```
CTRL + X
```

---

### View file

```bash
cat file.txt
```

Large file:

```bash
less file.txt
```

---

### View logs live

Very useful for production:

```bash
tail -f log.txt
```

Example for Nginx:

```bash
tail -f /var/log/nginx/error.log
```

---

# 5️⃣ Server Service Management

Ubuntu uses **systemd**.

---

### Check service status

Example nginx:

```bash
sudo systemctl status nginx
```

---

### Restart service

```bash
sudo systemctl restart nginx
```

---

### Start service

```bash
sudo systemctl start nginx
```

---

### Stop service

```bash
sudo systemctl stop nginx
```

---

# 6️⃣ Monitoring Node / PM2 Apps (Your Setup)

Since you run **Next.js with PM2**:

### Show processes

```bash
pm2 list
```

---

### Monitor

```bash
pm2 monit
```

Shows:

* CPU
* memory
* logs

---

### Logs

```bash
pm2 logs
```

---

# 7️⃣ Real Production Monitoring Stack

Basic commands are good, but production usually uses tools like:

Common tools:

```
htop        CPU monitoring
pm2 monit   Node apps
df -h       disk monitoring
tail -f     logs
```

Advanced monitoring:

* Prometheus
* Grafana
* Netdata
* Datadog
* New Relic

---

# 8️⃣ Daily DevOps Command Routine (Typical)

When logging into VPS:

```bash
ssh prod
```

Check health:

```bash
htop
df -h
pm2 list
```

Check logs:

```bash
pm2 logs
tail -f /var/log/nginx/error.log
```

Deploy:

```bash
git pull
npm run build
pm2 restart app
```

