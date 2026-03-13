# 💾 Automated Database Backups

Never lose your data. Set up automated daily backups to keep your production database safe.

## 1️⃣ Simple MySQL Backup Script
Create a script at `/home/ubuntu/backup.sh`:

```bash
#!/bin/bash

# Configuration
DB_NAME="your_database_name"
BACKUP_DIR="/home/ubuntu/backups"
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
MAP_FILE="$BACKUP_DIR/$DB_NAME_$TIMESTAMP.sql"

# Create backup directory if not exists
mkdir -p $BACKUP_DIR

# Run mysqldump
mysqldump -u root $DB_NAME > $MAP_FILE

# Compress
gzip $MAP_FILE

# Delete backups older than 7 days
find $BACKUP_DIR -type f -name "*.gz" -mtime +7 -delete
```

---

## 2️⃣ Make it Executable
```bash
chmod +x /home/ubuntu/backup.sh
```

---

## 3️⃣ Schedule with Cron
Open the crontab editor:
```bash
crontab -e
```

Add this line to run the backup every night at 2:00 AM:
```bash
0 2 * * * /home/ubuntu/backup.sh
```

---

## ☁️ Offsite Backups (Recommended)
Storing backups on the same server is risky. Use `rclone` or `aws-cli` to sync your backup directory to S3 or Google Drive:

```bash
# Example rsync to S3
aws s3 sync /home/ubuntu/backups s3://my-backup-bucket
```

---

## ✅ Backup Checklist
- [ ] Test restoring the backup at least once a month.
- [ ] Monitor the disk space of your backup directory.
- [ ] Use encrypted backups for sensitive data.
