# Yet Another One To-Do App!

This **To-Do App** is a user-friendly task management tool designed to help individuals organize their daily activities efficiently. 
With a clean and intuitive interface, users can easily add and delete tasks, ensuring that nothing important is overlooked.

Key Features:
- Task Management: Users can create tasks with titles and descriptions, allowing for detailed tracking of responsibilities.
- Customizable Appearance: The application offers settings to customize background colors, button colors, and secondary colors, enabling users to personalize their experience.
- Persistent Storage: Tasks are saved to a local file, ensuring that users' lists are preserved even after closing the application.
- User-Friendly Interface: The layout is designed for ease of use, with clearly labeled buttons and input fields for quick task entry.
- Settings Management: Users can easily access a settings window to adjust the application's appearance or restore default settings.
- Visual Feedback: The application provides visual cues and warnings to guide users, such as alerts for invalid task entries or reminders to select a task before deletion.

Whether you're managing personal errands, work projects, or study schedules, the To-Do List Application is a versatile tool that helps you stay organized and productive.

# How to use

1) Download the code:
```
git clone https://github.com/triple0ero/todo_app.git
```
3) Install dependencies:
```
pip install -r requirements.txt
```
3) Start the app:
```
python3 main.py
```
4) Enjoy!

apt install rsync

sudo nano /usr/local/bin/backup_share.sh

#!/bin/bash

# Configuration
SOURCE_DIR="/mnt/share"
BACKUP_DIR="/mnt/share_backup"
LOG_FILE="/var/log/share_backup.log"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Log function
log() {
    echo "[$TIMESTAMP] $1" >> "$LOG_FILE"
}

# Start backup
log "Starting backup from $SOURCE_DIR to $BACKUP_DIR"

# Perform rsync backup
rsync -av --delete \
      --exclude='*.tmp' \
      --exclude='.Trash*' \
      "$SOURCE_DIR/" "$BACKUP_DIR/" >> "$LOG_FILE" 2>&1

# Check if rsync was successful
if [ $? -eq 0 ]; then
    log "Backup completed successfully"
else
    log "BACKUP FAILED! Check for errors above"
    # You can add email notification here if needed
fi

log "Backup process finished"

sudo chmod +x /usr/local/bin/backup_share.sh

sudo crontab -e

*/30 * * * * /usr/local/bin/backup_share.sh

