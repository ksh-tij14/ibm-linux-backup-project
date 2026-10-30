# IBM Linux Commands and Shell Scripting - Final Project

Automated directory backup shell script created as part of the **Hands-on Introduction to Linux Commands and Shell Scripting** course by IBM on Coursera / IBM Skills Network.

## Overview

The `backup.sh` script automates the process of identifying files modified within the past 24 hours in a target directory, packaging and compressing them into a `.tar.gz` archive with a timestamped filename, and moving the archive to a designated destination directory. It is scheduled to execute automatically every 24 hours using `cron`.

---

## Features

- **Input Validation**: Verifies that exactly two command-line arguments are provided and validates that both paths exist and are valid directories.
- **Timestamping**: Captures current Unix timestamp in seconds (`date +%s`) for unique archive naming (`backup-[TIMESTAMP].tar.gz`).
- **Differential Backup**: Scans the target directory and filters for files modified within the last 24 hours (`86,400 seconds`).
- **Archiving & Compression**: Uses `tar -czvf` to bundle and compress only the updated files.
- **Automation Ready**: Designed for execution via cron job schedules.

---

## Script Architecture (`backup.sh`)

| Step | Description |
| :--- | :--- |
| **Argument Check** | Confirms 2 arguments: `targetDirectory` and `destinationDirectory`. |
| **Directory Validation** | Validates target and destination directories using `-d`. |
| **Task 1** | Assigns `$1` to `targetDirectory` and `$2` to `destinationDirectory`. |
| **Task 2** | Displays paths to the user. |
| **Task 3** | Sets `currentTS=$(date +%s)`. |
| **Task 4** | Sets `backupFileName="backup-$currentTS.tar.gz"`. |
| **Task 5** | Captures working directory: `origAbsPath=$(pwd)`. |
| **Task 6** | Navigates to `destinationDirectory` to capture `destAbsPath=$(pwd)`. |
| **Task 7** | Returns to `origAbsPath` and enters `targetDirectory`. |
| **Task 8** | Calculates `yesterdayTS=$(($currentTS - 24 * 60 * 60))`. |
| **Task 9–11** | Iterates through `$(ls)`, compares timestamps (`date -r $file +%s > $yesterdayTS`), and collects updated files into `toBackup` array. |
| **Task 12** | Creates compressed tar archive: `tar -czvf $backupFileName ${toBackup[@]}`. |
| **Task 13** | Moves archive to destination: `mv $backupFileName $destAbsPath`. |

---

## Usage

### 1. Make the script executable
```bash
chmod +x backup.sh
```

### 2. Run the backup script manually
```bash
./backup.sh /path/to/targetDirectory /path/to/destinationDirectory
```

### 3. Schedule via Crontab (Runs daily at midnight)
Open your user crontab editor:
```bash
crontab -e
```

Add the following cron expression:
```cron
0 0 * * * /home/project/backup.sh /home/project/targetDirectory /home/project/destinationDirectory
```

Verify scheduled cron jobs:
```bash
crontab -l
```

---

## License
MIT License
