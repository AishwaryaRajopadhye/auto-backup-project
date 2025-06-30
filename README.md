# 🔄 Automated Backup and Rotation Script with Google Drive Integration

This project automates the process of backing up a directory on a Linux server (e.g., AWS EC2). It zips the target directory, uploads it to Google Drive using `rclone`, and applies retention rules to manage storage space. Additionally, it supports webhook notifications to inform about backup status.

---

## 🧾 Overview

- 📦 Creates scheduled, timestamped backups of a project folder
- ☁️ Uploads backups to Google Drive via `rclone`
- 🔁 Enforces smart backup rotation (daily, weekly, monthly)
- 🔔 Sends optional webhook notifications (e.g., to Slack, Discord, or webhook.site)
- 🕒 Supports automated cron job scheduling

---

## ⚙️ How to Install & Configure `rclone`

1. **Install rclone**

```bash
curl https://rclone.org/install.sh | sudo bash
```
2. **Run configuration wizard**
```bash
   rclone config
```
3. **Create a new remote named gdrive:**
   
- Choose n (new remote)
- Name: gdrive
- Choose drive as the storage (usually option 18)
- Use n for auto config (headless setup)
- On your local machine, run:
```bash
 rclone authorize "drive"
```
Paste the resulting token JSON into your EC2 config prompt.

4. **Test upload**
```bash
echo "hello" > test.txt
rclone copy test.txt gdrive:
```
Check Google Drive for test.txt.

## 🔧 How to Configure Retention Settings

Edit the .env file in your project folder:

```env
PROJECT_NAME=MyProject
PROJECT_DIR=/home/ubuntu/MyProject

BACKUP_DIR=/home/ubuntu/auto-backup-project/backups

RETENTION_DAYS=7
RETENTION_WEEKS=4
RETENTION_MONTHS=3

RCLONE_REMOTE=gdrive
RCLONE_FOLDER=EC2Backups

LOG_FILE=/home/ubuntu/auto-backup-project/logs/backup.log

NOTIFY_URL=https://webhook.site/your-unique-url
ENABLE_NOTIFY=true
```
## 🧾 Python Backup Script (backup.py)
This is the core script that handles backup creation, rotation, Google Drive upload, logging, and webhook notifications.

Paste code from backup.py file.

## ✅ Run the Script Manually

- Make the script executable
```bash
chmod +x backup.py
```
-  Run the script
```bash
python3 backup.py
```
- Optional:
```bash
python3 backup.py --no-notify
```
## ⏲️ Automate with Cron

To run the backup every day at 2:00 AM:

1. Open Crontab
```bash
crontab -e
```
Choose nano as the editor (if prompted).

2. Add Cron Job Entry
```bash
0 2 * * * /home/ubuntu/auto-backup-project/venv/bin/python3 /home/ubuntu/auto-backup-project/backup.py >> /home/ubuntu/auto-backup-project/logs/cron.log 2>&1
```
3. Save and Exit
In nano:
```bash
Press Ctrl + O, then Enter to save
Press Ctrl + X to exit
```
You should see:
```bash
crontab: installing new crontab
```
4. Verify Cron is Set
```bash
crontab -l
```

