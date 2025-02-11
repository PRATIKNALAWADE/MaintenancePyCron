﻿# MaintenancePyCron
### **Use Case: Scripting for Routine Tasks and Maintenance**

Routine maintenance tasks like backups, system updates, and log rotation are essential for keeping your infrastructure healthy. You can automate these tasks using Python scripts and schedule them with cron jobs. Below are examples of Python scripts for common routine maintenance tasks and how to set them up with cron.

### **Example: Python Scripts for Routine Tasks**

#### **1. Backup Script**

**Scenario:**
Create a Python script to back up a directory to a backup location. This script will be scheduled to run daily to ensure that your data is regularly backed up.

**Backup Script (`backup_script.py`):**

```python
import shutil
import os
from datetime import datetime

# Define source and backup directories
source_dir = '/path/to/source_directory'
backup_dir = '/path/to/backup_directory'

# Create a timestamped backup file name
timestamp = datetime.now().strftime('%Y%m%d-%H%M%S')
backup_file = f'{backup_dir}/backup_{timestamp}.tar.gz'

def create_backup():
    """Create a backup of the source directory."""
    shutil.make_archive(backup_file.replace('.tar.gz', ''), 'gztar', source_dir)
    print(f'Backup created at {backup_file}')

if __name__ == '__main__':
    create_backup()
```

#### **2. System Update Script**

**Scenario:**
Create a Python script to update the system packages. This script will ensure that the system is kept up-to-date with the latest security patches and updates.

**System Update Script (`system_update.py`):**

```python
import subprocess

def update_system():
    """Update the system packages."""
    try:
        subprocess.run(['sudo', 'apt-get', 'update'], check=True)
        subprocess.run(['sudo', 'apt-get', 'upgrade', '-y'], check=True)
        print('System updated successfully.')
    except subprocess.CalledProcessError as e:
        print(f'Failed to update the system: {e}')

if __name__ == '__main__':
    update_system()
```

#### **3. Log Rotation Script**

**Scenario:**
Create a Python script to rotate log files, moving old logs to an archive directory and compressing them.

**Log Rotation Script (`log_rotation.py`):**

```python
import os
import shutil
from datetime import datetime

# Define log directory and archive directory
log_dir = '/path/to/log_directory'
archive_dir = '/path/to/archive_directory'

def rotate_logs():
    """Rotate log files by moving and compressing them."""
    for log_file in os.listdir(log_dir):
        log_path = os.path.join(log_dir, log_file)
        if os.path.isfile(log_path):
            timestamp = datetime.now().strftime('%Y%m%d-%H%M%S')
            archive_file = f'{archive_dir}/{log_file}_{timestamp}.gz'
            shutil.copy(log_path, archive_file)
            shutil.make_archive(archive_file.replace('.gz', ''), 'gztar', root_dir=archive_dir, base_dir=log_file)
            os.remove(log_path)
            print(f'Log rotated: {archive_file}')

if __name__ == '__main__':
    rotate_logs()
```

### **Setting Up Cron Jobs**

You need to set up cron jobs to schedule these scripts to run at specific intervals. Use the `crontab` command to edit the cron schedule.

1. **Open the Crontab File:**

   ```bash
   crontab -e
   ```

2. **Add Cron Job Entries:**

   - **Daily Backup at 2 AM:**

     ```bash
     0 2 * * * /usr/bin/python3 /path/to/backup_script.py
     ```

   - **Weekly System Update on Sunday at 3 AM:**

     ```bash
     0 3 * * 0 /usr/bin/python3 /path/to/system_update.py
     ```

   - **Log Rotation Every Day at Midnight:**

     ```bash
     0 0 * * * /usr/bin/python3 /path/to/log_rotation.py
     ```

**Explanation:**

- `0 2 * * *`: Runs the script at 2:00 AM every day.
- `0 3 * * 0`: Runs the script at 3:00 AM every Sunday.
- `0 0 * * *`: Runs the script at midnight every day.

### **Conclusion**

Using Python scripts for routine tasks and maintenance helps automate critical processes such as backups, system updates, and log rotation. By scheduling these scripts with cron jobs, you ensure that these tasks are performed consistently and without manual intervention. This approach enhances the reliability and stability of your infrastructure, keeping it healthy and up-to-date.
