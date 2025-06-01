# 100-days-of-cybersecurity
I'll be documenting what I learn each day, tools I explore, and challenges I face.  
Follow along and let’s grow together! 💻

## 🔰 Challenge Goal:
- Learn cybersecurity fundamentals and hands-on tools
- Build consistency and practical experience
- Share progress to stay accountable

---

## 📅 Daily Logs

### ✅ Day 1 – Installing Linux & Basic Commands
- Installed **Ubuntu** and **Kali Linux** on VMware Fusion
- Practiced navigation commands: `cd`, `pwd`, `ls -lh`, etc.
- Explored `stat`, `du -sh`, `df -h` for system insights
- Learned `grep` for searching text
- Played with **brace expansion** (`file{1..3}.txt`) and **globbing** (`*.txt`)
- Edited files using `vim` for the first time!

> “Linux is powerful — and now I see why every cybersecurity expert swears by the terminal.” 💪

---

### ✅ Day 2 – Linux Users, Groups & Permissions

Today’s focus was on understanding how Linux handles users, groups, and permissions.

#### 🧠 Key Concepts Covered:

- **User Types:**
  - Regular users
  - System users
  - Sudo (admin) users

- **Switching to Root:**
  - Used: `sudo su -`

- **Creating Groups and Users:**
  - Created a group: `groupadd analysts`
  - Created user with home & group: `useradd -m -G analysts username`
  - Noticed that without `-m`, no home directory is created
  - Verified shell with: `tail -n /etc/passwd`
  - Changed shell using: `chsh -s /bin/bash username`

- **Permissions Management:**
  - Used `chown` to change file ownership
  - Used `chmod` for permissions:
    - Alphabetically: `u+x`, `g-w`
    - Numerically: `chmod 755`
  - Explored:
    - Default permissions using `umask`
    - Special permissions (setuid, setgid, sticky bit)

---

> Every command I used today deepened my understanding of how Linux enforces access control — a key skill in system hardening and cybersecurity.

---
### 📅 Day 3 of 100 – Bash Scripting Challenge

## 🎯 Task
- Create a Bash script to list all users and display their last login time.

## 🔍 Objective
- To deepen my understanding of Bash scripting, user account structure in Linux, and system auditing using command-line tools.

## 🧠 What I Learned
- How to read and parse /etc/passwd
- Filtering regular users (UID ≥ 1000)
- Using the last command to fetch login records
- Handling null or missing login entries
- Basics of:
- Conditional logic (if, else)
- Loops (while read)
- Shell variables
- Command substitution ($(...))

## ⚙️ Script Summary
- The script:
- Prints the current time
- Iterates through users
- Skips system users and incomplete records
- Displays user details and last login information
-Alerts if the root user is found

## 🚧 Challenge
- This was my first real Bash scripting experience. It was confusing, overwhelming, and required a lot of Googling and testing. I didn't understand everything right away, but I kept pushing through.

## ✅ Key Commands Used
- last
- grep
- head
- read
- if/else
- echo

## 💬 Reflection
This day tested my patience, but also opened my eyes to how scripting automates auditing. I still have a long way to go, but this is a solid step forward. I may have posted this late, but I’m proud I didn’t give up.


# 📆 Day 4 of 100 Days of Cybersecurity

## 🔄 Automating Log Cleanup with Bash and Cron

In today's task, I worked on automating log file cleanups using **Bash scripting** and **cron jobs**. I explored two approaches:

---

## ✅ Approach 1: System-Level Log Cleanup (Using `sudo`)

**🗂️ Directory:** `/var/log`  
**🕒 Retention:** Logs older than **12 days**  
**🛑 Exceptions:** `auth.log`, `syslog` were excluded from deletion.

### 🔧 Script Snippet

```bash
#!/bin/bash

LOG_DIR="/var/log"
RETENTION_DAYS=12

find "$LOG_DIR" -type f -name "*.log" -mtime +$RETENTION_DAYS \
! -name "auth.log" ! -name "syslog" -print -exec rm -f {} \;

echo "$(date): /var/log cleanup completed." >> /var/log/cleanup_record.log

```
📅 Cron Job (runs daily at 15pm)

``` sudo crontab -e
# Add this line:
1 5 * * * /path/to/cleanup_script.sh
```

## 🧪 Approach 2: User-Level Log Cleanup (No sudo)

To test log deletion as a non-root user, I created a test log directory in my home folder.

🗂️ Directory: ~/sys_logs
🕒 Retention: Logs older than 0 days (for instant test)
🗒️ Log Record: Stored in ~/sys_logs_record.txt

### 🔧 Script Snippet
```bash
#!/bin/bash

LOG_DIR="$HOME/sys_logs"
RETENTION_DAYS=0

find "$LOG_DIR" -type f -name "*.log" -mtime +$RETENTION_DAYS -exec rm -f {} \;

echo "$(date): Removed logs older than $RETENTION_DAYS in $LOG_DIR" >> "$HOME/sys_logs_record.txt"
```
### 📅 Cron Job (runs daily at 10AM)
```
crontab -e
# Add this line:
1 0 * * * /home/yourusername/sys_logs.sh
```
## 📚 What I Learned

- Writing Bash scripts using find, -exec, -print
- Differentiating between root-level and user-level permissions
- Automating system tasks with cron
- Logging cleanup actions for auditing purposes
