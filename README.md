# Cleanup Old Files Script

A simple Bash script for automatically removing old files and empty directories from a specified location.  
Useful for cleaning up archives, recordings, backups, or other large storage directories in Linux environments.

---

## 🧰 Features

- Deletes files older than a specified number of days  
- Removes empty directories after cleanup  
- Logs all actions (including deleted files and folders)  
- Supports multiple subdirectories  
- Lightweight, portable, and easy to schedule with `cron`

---

## ⚙️ Configuration

Edit the variables at the top of the script as needed:

```bash
#!/bin/bash
#
# Cleanup old files and empty directories (real delete version)
# 
# This script removes files older than a specified number of days
# and deletes any empty directories afterwards. It logs all actions.
#

# === Configuration ===
TARGET_DIR="/path/to/storage"           # Base directory to clean
LOG_FILE="/var/log/cleanup.log"         # Log file for cleanup actions
DAYS_OLD=30                             # Number of days to keep files
FOLDERS=("folder1" "folder2" "folder3") # Subdirectories to process

# === Start ===
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Starting cleanup in $TARGET_DIR" >> "$LOG_FILE"

for folder in "${FOLDERS[@]}"; do
  FOLDER_PATH="$TARGET_DIR/$folder"

  # Skip if folder doesn’t exist
  [ -d "$FOLDER_PATH" ] || continue

  echo "[$(date '+%Y-%m-%d %H:%M:%S')] Cleaning $FOLDER_PATH (older than $DAYS_OLD days)" >> "$LOG_FILE"

  # Delete old files and log each one
  find "$FOLDER_PATH" -type f -mtime +$DAYS_OLD -print -delete >> "$LOG_FILE" 2>&1

  # Delete empty directories and log each one
  find "$FOLDER_PATH" -type d -empty -print -delete >> "$LOG_FILE" 2>&1
done

echo "[$(date '+%Y-%m-%d %H:%M:%S')] Cleanup finished — old files and empty directories removed" >> "$LOG_FILE"
```
- folder is just a loop variable — it could be named anything.
You can think of it as a placeholder like folder, dir, or item. <br>
- Variable DAYS_OLD = Sets how many days old a file must be before it’s considered for deletion.<br>
For example, DAYS_OLD=30 means any file older than 30 days will be removed.<br>

🚀 Usage
- Run manually or schedule with cron.

🔒 Safety Tips
- Test first by removing -delete from find to preview what would be removed.<br>
- Ensure log file path is writable and secured. <br>
- Run as a dedicated user or via cron with restricted permissions.<br>

🧑‍💻 About

This small utility script demonstrates:
- Bash scripting and filesystem management <br>
- Safe cleanup automation <br>
- Use of find, logging, and date-based file operations <br>
- Practical system administration mindset <br>
- Created as part of my personal portfolio to showcase Linux automation and maintenance scripting skills. <br>
