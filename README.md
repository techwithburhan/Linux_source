Here's a comprehensive list of essential Linux system commands:

## File and Directory Operations
- `ls` - list directory contents
- `ls -l` - long format listing
- `ls -a` - show hidden files
- `ls -lh` - human-readable file sizes
- `cd` - change directory
- `pwd` - print working directory
- `mkdir` - create directory
- `rmdir` - remove empty directory
- `rm` - remove files
- `rm -rf` - force remove directory and contents
- `cp` - copy files
- `cp -r` - copy directories recursively
- `mv` - move or rename files
- `touch` - create empty file or update timestamp
- `cat` - display file contents
- `less` - view file with pagination
- `more` - view file page by page
- `head` - show first lines of file
- `tail` - show last lines of file
- `tail -f` - follow file updates in real-time

## System Information
- `uname -a` - system information
- `hostname` - show hostname
- `uptime` - system uptime
- `date` - current date and time
- `cal` - calendar
- `whoami` - current username
- `id` - user and group IDs
- `last` - last logged-in users
- `w` - who is logged in and what they're doing
- `who` - show logged-in users

## Disk and Filesystem
- `df -h` - disk space usage (human-readable)
- `df -i` - inode usage
- `du -h` - directory space usage
- `du -sh` - summarize directory size
- `mount` - show mounted filesystems
- `umount` - unmount filesystem
- `lsblk` - list block devices
- `fdisk -l` - list disk partitions
- `blkid` - locate/print block device attributes

## Process Management
- `ps` - process status
- `ps aux` - all processes detailed view
- `ps -ef` - all processes full format
- `top` - real-time process monitor
- `htop` - interactive process viewer (if installed)
- `kill` - terminate process by PID
- `killall` - kill processes by name
- `pkill` - kill processes by pattern
- `pgrep` - find process IDs by name
- `jobs` - list background jobs
- `bg` - resume job in background
- `fg` - bring job to foreground
- `nohup` - run command immune to hangups

## Memory and Performance
- `free -h` - memory usage (human-readable)
- `vmstat` - virtual memory statistics
- `iostat` - CPU and I/O statistics
- `mpstat` - CPU statistics

## Network Commands
- `ifconfig` - network interface configuration
- `ip addr` - show IP addresses
- `ip link` - show network interfaces
- `ping` - test network connectivity
- `traceroute` - trace route to host
- `netstat` - network statistics
- `netstat -tulpn` - listening ports
- `ss` - socket statistics
- `ss -tulpn` - listening TCP/UDP ports
- `nslookup` - DNS lookup
- `dig` - DNS query tool
- `host` - DNS lookup
- `wget` - download files
- `curl` - transfer data from URLs
- `route` - show routing table
- `arp` - ARP cache

## User Management
- `useradd` - add user
- `userdel` - delete user
- `usermod` - modify user
- `passwd` - change password
- `groupadd` - add group
- `groupdel` - delete group
- `groups` - show user groups
- `sudo` - execute as superuser
- `su` - switch user

## File Permissions
- `chmod` - change file permissions
- `chown` - change file owner
- `chgrp` - change group ownership
- `umask` - set default permissions

## Search and Find
- `find` - search for files
- `locate` - find files by name
- `which` - locate command
- `whereis` - locate binary, source, and man page
- `grep` - search text patterns
- `grep -r` - recursive search
- `egrep` - extended grep
- `fgrep` - fixed string grep

## Archive and Compression
- `tar -cvf` - create tar archive
- `tar -xvf` - extract tar archive
- `tar -czvf` - create gzipped tar
- `tar -xzvf` - extract gzipped tar
- `gzip` - compress files
- `gunzip` - decompress gzip files
- `zip` - create zip archive
- `unzip` - extract zip archive
- `bzip2` - compress with bzip2
- `bunzip2` - decompress bzip2

## System Control
- `systemctl start` - start service
- `systemctl stop` - stop service
- `systemctl restart` - restart service
- `systemctl status` - check service status
- `systemctl enable` - enable service at boot
- `systemctl disable` - disable service at boot
- `service` - manage services (older systems)
- `shutdown` - shutdown system
- `shutdown -h now` - halt immediately
- `shutdown -r now` - reboot immediately
- `reboot` - reboot system
- `poweroff` - power off system
- `init 0` - shutdown
- `init 6` - reboot

## Package Management (varies by distribution)

**Debian/Ubuntu (apt):**
- `apt update` - update package lists
- `apt upgrade` - upgrade packages
- `apt install` - install package
- `apt remove` - remove package
- `apt search` - search packages
- `dpkg -l` - list installed packages
- `dpkg -i` - install .deb package

**RHEL/CentOS/Fedora (yum/dnf):**
- `yum update` - update packages
- `yum install` - install package
- `yum remove` - remove package
- `dnf install` - install (newer systems)
- `rpm -qa` - list installed packages
- `rpm -i` - install .rpm package

## Log Files
- `dmesg` - kernel ring buffer messages
- `journalctl` - query systemd journal
- `journalctl -f` - follow journal logs
- `tail -f /var/log/syslog` - follow system log

## Hardware Information
- `lscpu` - CPU information
- `lshw` - hardware configuration
- `lspci` - PCI devices
- `lsusb` - USB devices
- `dmidecode` - DMI/SMBIOS information
- `hdparm` - disk parameters

## Text Processing
- `awk` - pattern scanning and processing
- `sed` - stream editor
- `cut` - cut sections from lines
- `sort` - sort lines
- `uniq` - remove duplicate lines
- `wc` - word, line, byte count
- `wc -l` - count lines
- `diff` - compare files
- `comm` - compare sorted files

## Miscellaneous
- `echo` - display text
- `history` - command history
- `alias` - create command aliases
- `clear` - clear terminal screen
- `man` - manual pages
- `info` - info documentation
- `help` - help for built-in commands
- `cron` - schedule tasks
- `crontab -e` - edit cron jobs
- `at` - schedule one-time task
- `watch` - execute program periodically
- `screen` - terminal multiplexer
- `tmux` - terminal multiplexer
- `xargs` - build command lines from input

These commands cover most daily system administration and user tasks in Linux. You can get detailed information about any command using `man command` or `command --help`.
# Linux User Management - Complete Guide & Hands-On Labs

## Linux Complete 
1. [Understanding Linux Users and Groups](#understanding-linux-users-and-groups)
2. [User Management Commands](#user-management-commands)
3. [Group Management](#group-management)
4. [File Permissions and Ownership](#file-permissions-and-ownership)
5. [Password Management](#password-management)
6. [Hands-On Labs](#hands-on-labs)

## Docker Complete 
1. [Understanding Linux Users and Groups](#understanding-linux-users-and-groups)
2. [User Management Commands](#user-management-commands)
3. [Group Management](#group-management)
4. [File Permissions and Ownership](#file-permissions-and-ownership)
5. [Password Management](#password-management)
6. [Hands-On Labs](#hands-on-labs)
---

## Understanding Linux Users and Groups

### Types of Users

**Root User (UID 0)**
- The superuser with complete system access
- Can perform any operation without restrictions
- Username is always `root`

**System Users (UID 1-999)**
- Created for running services and daemons
- Examples: apache, nginx, mysql
- Cannot login interactively (usually)
- No home directory or shell access

**Regular Users (UID 1000+)**
- Created for human users
- Have home directories (usually in /home)
- Can login and run applications
- Limited privileges

### Important Configuration Files

**`/etc/passwd`** - Stores user account information
```
username:x:UID:GID:GECOS:home_directory:shell
```
Example: `john:x:1001:1001:John Doe:/home/john:/bin/bash`

**`/etc/shadow`** - Stores encrypted passwords
```
username:encrypted_password:last_change:min:max:warn:inactive:expire
```

**`/etc/group`** - Stores group information
```
group_name:x:GID:members
```
Example: `developers:x:1005:john,alice,bob`

**`/etc/gshadow`** - Stores encrypted group passwords

**`/etc/login.defs`** - Default configuration for user creation

**`/etc/default/useradd`** - Default settings for useradd command

---

## User Management Commands

### Creating Users

**Basic user creation:**
```bash
useradd username
```

**Create user with options:**
```bash
useradd -m -d /home/username -s /bin/bash -c "Full Name" -G groupname username
```

**Options:**
- `-m` : Create home directory
- `-d` : Specify home directory path
- `-s` : Specify login shell
- `-c` : Add comment (full name/description)
- `-G` : Add to supplementary groups
- `-g` : Specify primary group
- `-u` : Specify UID
- `-e` : Set account expiry date (YYYY-MM-DD)
- `-f` : Set password inactive days after expiry

**Examples:**
```bash
# Create user with home directory
useradd -m john

# Create user with custom UID and home directory
useradd -m -u 1500 -s /bin/bash alice

# Create system user for service
useradd -r -s /sbin/nologin nginx

# Create user with expiry date
useradd -m -e 2024-12-31 tempuser
```

### Modifying Users

**Change user properties:**
```bash
usermod [options] username
```

**Common modifications:**
```bash
# Change username
usermod -l newname oldname

# Change home directory
usermod -d /new/home/path -m username

# Change shell
usermod -s /bin/zsh username

# Lock user account
usermod -L username

# Unlock user account
usermod -U username

# Add user to supplementary group
usermod -aG groupname username

# Change primary group
usermod -g newgroup username

# Set account expiry
usermod -e 2024-12-31 username
```

### Deleting Users

```bash
# Delete user (keep home directory)
userdel username

# Delete user and home directory
userdel -r username

# Force delete (even if logged in)
userdel -f username
```

### Viewing User Information

```bash
# Display user information
id username

# Show all logged-in users
who

# Show current user
whoami

# Display last login information
last username

# Show user's groups
groups username

# View password aging information
chage -l username

# List all users
cat /etc/passwd | cut -d: -f1

# List regular users only (UID >= 1000)
awk -F: '$3 >= 1000 {print $1}' /etc/passwd
```

---

## Group Management

### Creating Groups

```bash
# Create a new group
groupadd groupname

# Create group with specific GID
groupadd -g 2000 developers

# Create system group
groupadd -r sysgroup
```

### Modifying Groups

```bash
# Change group name
groupmod -n newname oldname

# Change GID
groupmod -g 3000 groupname

# Add user to group
usermod -aG groupname username

# Remove user from group
gpasswd -d username groupname
```

### Deleting Groups

```bash
groupdel groupname
```

### Managing Group Members

```bash
# Add multiple users to group
gpasswd -M user1,user2,user3 groupname

# Add single user to group
gpasswd -a username groupname

# Remove user from group
gpasswd -d username groupname

# Set group administrator
gpasswd -A admin_user groupname
```

---

## File Permissions and Ownership

### Understanding Permissions

**Permission types:**
- `r` (4) - Read
- `w` (2) - Write
- `x` (1) - Execute

**Permission groups:**
- User (u) - File owner
- Group (g) - Group owner
- Others (o) - Everyone else

**Example:**
```
-rwxr-xr-x  1 john developers 4096 Dec 20 10:30 script.sh
│││││││││
│││││││││
│││└└└└└└─ Others permissions (r-x = 5)
││└└└───── Group permissions (r-x = 5)
│└└└────── User permissions (rwx = 7)
└───────── File type (- = regular file)
```

### Changing Ownership

```bash
# Change file owner
chown username filename

# Change owner and group
chown username:groupname filename

# Change recursively
chown -R username:groupname directory/

# Change only group
chgrp groupname filename
```

### Changing Permissions

**Symbolic mode:**
```bash
# Add execute permission for user
chmod u+x filename

# Remove write permission for group
chmod g-w filename

# Set exact permissions
chmod u=rwx,g=rx,o=r filename

# Add read for all
chmod a+r filename
```

**Numeric mode:**
```bash
# rwxr-xr-x
chmod 755 filename

# rw-r--r--
chmod 644 filename

# rwx------
chmod 700 filename

# Recursive
chmod -R 755 directory/
```

### Special Permissions

**SUID (Set User ID) - 4**
- File executes with owner's permissions
```bash
chmod u+s filename
chmod 4755 filename
```

**SGID (Set Group ID) - 2**
- File executes with group's permissions
- New files in directory inherit group
```bash
chmod g+s directory
chmod 2775 directory
```

**Sticky Bit - 1**
- Only owner can delete files in directory
```bash
chmod +t directory
chmod 1777 directory
```

---

## Password Management

### Setting Passwords

```bash
# Set/change password for current user
passwd

# Set password for another user (root only)
passwd username

# Set password non-interactively
echo "newpassword" | passwd --stdin username
```

### Password Aging

```bash
# View password aging information
chage -l username

# Set password expiry date
chage -E 2024-12-31 username

# Set minimum days between password changes
chage -m 7 username

# Set maximum days password is valid
chage -M 90 username

# Set warning days before password expires
chage -W 14 username

# Set inactive days after expiry
chage -I 30 username

# Force password change on next login
chage -d 0 username

# Interactive mode
chage username
```

### Password Policies

**Edit `/etc/login.defs`:**
```bash
PASS_MAX_DAYS   90      # Maximum password age
PASS_MIN_DAYS   7       # Minimum password age
PASS_MIN_LEN    8       # Minimum password length
PASS_WARN_AGE   14      # Password expiration warning
```

### Locking/Unlocking Accounts

```bash
# Lock user account
passwd -l username
usermod -L username

# Unlock user account
passwd -u username
usermod -U username

# Check account status
passwd -S username
```

---

## Hands-On Labs

### Lab 1: Basic User Creation and Management

**Objective:** Create users, set passwords, and manage basic properties

**Tasks:**

1. Create three users: alice, bob, and charlie
```bash
sudo useradd -m -s /bin/bash alice
sudo useradd -m -s /bin/bash bob
sudo useradd -m -s /bin/bash charlie
```

2. Set passwords for all users
```bash
sudo passwd alice
sudo passwd bob
sudo passwd charlie
```

3. Verify users were created
```bash
tail -3 /etc/passwd
id alice
id bob
id charlie
```

4. Check home directories
```bash
ls -la /home/
```

5. Modify alice's comment field
```bash
sudo usermod -c "Alice Smith - Developer" alice
grep alice /etc/passwd
```

6. Change bob's shell to zsh (if available)
```bash
sudo usermod -s /bin/zsh bob
grep bob /etc/passwd
```

**Verification:**
```bash
# Check user details
id alice
getent passwd alice
groups alice
```

---

### Lab 2: Group Management and Membership

**Objective:** Create groups and manage user memberships

**Tasks:**

1. Create groups: developers, testers, managers
```bash
sudo groupadd developers
sudo groupadd testers
sudo groupadd managers
```

2. Verify groups were created
```bash
tail -3 /etc/group
getent group developers
```

3. Add users to groups
```bash
sudo usermod -aG developers alice
sudo usermod -aG developers,testers bob
sudo usermod -aG managers charlie
```

4. Verify group memberships
```bash
groups alice
groups bob
groups charlie
id -Gn alice
```

5. Create a new user and add to multiple groups at creation
```bash
sudo useradd -m -G developers,testers david
groups david
```

6. Remove bob from testers group
```bash
sudo gpasswd -d bob testers
groups bob
```

7. Change group name
```bash
sudo groupmod -n engineering developers
getent group engineering
```

**Verification:**
```bash
# List all members of engineering group
getent group engineering
# Check user's groups
id alice
```

---

### Lab 3: File Permissions and Ownership

**Objective:** Practice setting file permissions and ownership

**Tasks:**

1. Create a test directory and files
```bash
sudo su - alice
mkdir ~/project
cd ~/project
touch file1.txt file2.sh file3.conf
ls -l
```

2. Set different permissions
```bash
# Regular file - rw-r--r--
chmod 644 file1.txt

# Executable script - rwxr-xr-x
chmod 755 file2.sh

# Config file - rw-r-----
chmod 640 file3.conf

ls -l
```

3. Change ownership
```bash
exit  # Back to sudo user
sudo chown alice:engineering /home/alice/project/file1.txt
sudo chown bob:testers /home/alice/project/file2.sh
ls -l /home/alice/project/
```

4. Create a shared directory
```bash
sudo mkdir /shared/team_project
sudo chgrp engineering /shared/team_project
sudo chmod 770 /shared/team_project
ls -ld /shared/team_project
```

5. Set SGID on shared directory
```bash
sudo chmod g+s /shared/team_project
ls -ld /shared/team_project
# New files will inherit the group
```

6. Test SGID functionality
```bash
sudo su - alice
touch /shared/team_project/test_file
ls -l /shared/team_project/test_file
# Should show 'engineering' as group
exit
```

7. Create a temporary directory with sticky bit
```bash
sudo mkdir /shared/temp
sudo chmod 1777 /shared/temp
ls -ld /shared/temp
# Only file owners can delete their files
```

**Verification:**
```bash
# Check permissions
ls -l /home/alice/project/
stat /shared/team_project
```

---

### Lab 4: Password Policies and Account Management

**Objective:** Implement password policies and manage account lifecycle

**Tasks:**

1. View current password aging for alice
```bash
sudo chage -l alice
```

2. Set password aging policies
```bash
# Password expires in 90 days
sudo chage -M 90 alice

# Minimum 7 days between changes
sudo chage -m 7 alice

# Warning 14 days before expiry
sudo chage -W 14 alice

# Account inactive 30 days after password expires
sudo chage -I 30 alice
```

3. Verify changes
```bash
sudo chage -l alice
```

4. Force password change on next login
```bash
sudo chage -d 0 bob
```

5. Set account expiration date
```bash
sudo chage -E 2025-06-30 charlie
sudo chage -l charlie
```

6. Lock and unlock accounts
```bash
# Lock alice's account
sudo passwd -l alice
sudo passwd -S alice  # Check status

# Unlock alice's account
sudo passwd -u alice
sudo passwd -S alice
```

7. Create a temporary account with expiration
```bash
sudo useradd -m -e 2025-01-31 tempuser
sudo passwd tempuser
sudo chage -l tempuser
```

8. View last login information
```bash
last alice
lastlog
```

**Verification:**
```bash
# Check password status for all users
sudo passwd -S alice
sudo passwd -S bob
sudo chage -l charlie
```

---

### Lab 5: Advanced User Management Scenarios

**Objective:** Handle complex user management scenarios

**Tasks:**

1. Create department structure
```bash
# Create departments
sudo groupadd -g 3001 hr
sudo groupadd -g 3002 sales
sudo groupadd -g 3003 it

# Create shared directories
sudo mkdir -p /shared/{hr,sales,it}

# Set ownership and permissions
sudo chgrp hr /shared/hr
sudo chgrp sales /shared/sales
sudo chgrp it /shared/it

sudo chmod 2770 /shared/hr
sudo chmod 2770 /shared/sales
sudo chmod 2770 /shared/it
```

2. Create users for each department
```bash
# HR users
sudo useradd -m -G hr -c "HR Employee 1" hr_user1
sudo useradd -m -G hr -c "HR Employee 2" hr_user2

# Sales users
sudo useradd -m -G sales -c "Sales Rep 1" sales_user1
sudo useradd -m -G sales -c "Sales Rep 2" sales_user2

# IT users
sudo useradd -m -G it -c "IT Admin 1" it_user1
sudo useradd -m -G it -c "IT Admin 2" it_user2
```

3. Create a cross-functional project group
```bash
sudo groupadd project_alpha
sudo usermod -aG project_alpha hr_user1
sudo usermod -aG project_alpha sales_user1
sudo usermod -aG project_alpha it_user1

sudo mkdir /shared/project_alpha
sudo chgrp project_alpha /shared/project_alpha
sudo chmod 2770 /shared/project_alpha
```

4. Set up sudo access for IT group
```bash
# Add IT group to sudoers
sudo visudo
# Add line: %it ALL=(ALL) ALL
```

5. Create system service user
```bash
sudo useradd -r -s /sbin/nologin -d /var/lib/myapp myapp
sudo mkdir /var/lib/myapp
sudo chown myapp:myapp /var/lib/myapp
```

6. Audit user accounts
```bash
# List all regular users
awk -F: '$3 >= 1000 && $1 != "nobody" {print $1}' /etc/passwd

# List users with no password set
sudo awk -F: '($2 == "" || $2 == "!!" || $2 == "*") {print $1}' /etc/shadow

# Find users with UID 0 (should only be root)
awk -F: '$3 == 0 {print $1}' /etc/passwd
```

**Verification:**
```bash
# Check directory permissions
ls -ld /shared/*

# Verify group memberships
getent group project_alpha

# Check service user
id myapp
```

---

### Lab 6: User Account Cleanup and Reporting

**Objective:** Generate reports and clean up user accounts

**Tasks:**

1. Generate user report
```bash
# Create a script to list all users with details
cat > user_report.sh << 'EOF'
#!/bin/bash
echo "User Account Report - $(date)"
echo "================================"
echo ""
awk -F: '$3 >= 1000 {printf "Username: %-15s UID: %-6s Home: %-25s Shell: %s\n", $1, $3, $6, $7}' /etc/passwd
EOF

chmod +x user_report.sh
./user_report.sh
```

2. Find inactive users (not logged in for 90 days)
```bash
lastlog -b 90
```

3. List locked accounts
```bash
sudo passwd -S -a | grep " L "
```

4. Find accounts without passwords
```bash
sudo awk -F: '($2 == "" || $2 == "!!") {print $1}' /etc/shadow
```

5. Check for duplicate UIDs
```bash
awk -F: '{print $3}' /etc/passwd | sort | uniq -d
```

6. List users by group
```bash
for group in $(cut -d: -f1 /etc/group); do
    echo "Group: $group"
    members=$(getent group $group | cut -d: -f4)
    if [ -n "$members" ]; then
        echo "  Members: $members"
    else
        echo "  Members: none"
    fi
    echo ""
done
```

7. Delete test users
```bash
# Remove users created in labs
for user in alice bob charlie david tempuser hr_user1 hr_user2 sales_user1 sales_user2 it_user1 it_user2; do
    sudo userdel -r $user 2>/dev/null
done

# Remove groups
for group in developers testers managers engineering hr sales it project_alpha; do
    sudo groupdel $group 2>/dev/null
done
```

**Verification:**
```bash
# Verify cleanup
cat /etc/passwd | grep -E "alice|bob|charlie"
getent group developers
```

---

## Quick Reference Commands

### User Commands
```bash
useradd          # Create user
usermod          # Modify user
userdel          # Delete user
passwd           # Set/change password
chage            # Change password aging
id               # Display user info
whoami           # Show current user
who              # Show logged-in users
w                # Show who is logged in and what they're doing
last             # Show login history
```

### Group Commands
```bash
groupadd         # Create group
groupmod         # Modify group
groupdel         # Delete group
gpasswd          # Administer groups
groups           # Show user's groups
newgrp           # Change current group
```

### Permission Commands
```bash
chmod            # Change file permissions
chown            # Change file owner
chgrp            # Change file group
umask            # Set default permissions
```

### Information Commands
```bash
getent passwd    # Query user database
getent group     # Query group database
cat /etc/passwd  # View all users
cat /etc/group   # View all groups
```

---

## Best Practices

1. **Always use `-m` with useradd** to create home directory
2. **Use strong password policies** - enforce complexity and aging
3. **Follow the principle of least privilege** - give users minimum necessary permissions
4. **Use groups for access control** - easier to manage than individual permissions
5. **Regular audits** - review user accounts and permissions periodically
6. **Document changes** - maintain records of user creation/modification
7. **Use sudo instead of root** - better accountability and logging
8. **Set account expiration** for temporary users
9. **Lock accounts instead of deleting** - preserves UID and files
10. **Back up** `/etc/passwd`, `/etc/shadow`, `/etc/group` before major changes

---

## Troubleshooting

**User cannot login:**
- Check if account is locked: `passwd -S username`
- Verify password expiry: `chage -l username`
- Check shell is valid: `cat /etc/passwd | grep username`

**Permission denied errors:**
- Verify file ownership: `ls -l filename`
- Check user's groups: `groups username`
- Review directory permissions: `ls -ld directory`

**Cannot create user:**
- Check for duplicate username/UID
- Verify you have sudo/root access
- Check disk space: `df -h`

**Group membership not taking effect:**
- User needs to log out and back in
- Or use `newgrp groupname` to activate immediately

# 🐧 Linux Interview Questions & Answers

> A comprehensive guide to the most frequently asked Linux interview questions for **Cloud Engineers**, **DevOps Engineers**, and **System Administrators**.

[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.linux.org/)
[![Shell Script](https://img.shields.io/badge/Shell_Script-121011?style=for-the-badge&logo=gnu-bash&logoColor=white)](https://www.gnu.org/software/bash/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)

---

## 📚 Table of Contents

- [Introduction](#introduction)
- [Basic Linux Concepts](#-basic-linux-concepts)
- [File & Directory Management](#-file--directory-management)
- [Permissions & Ownership](#-permissions--ownership)
- [Process & System Management](#-process--system-management)
- [Disk & File System](#-disk--file-system)
- [Networking](#-networking)
- [Package Management](#-package-management)
- [Logs & Troubleshooting](#-logs--troubleshooting)
- [Shell Scripting](#-shell-scripting)
- [Top 10 Most Asked Questions](#-top-10-most-asked-questions)
- [Additional Resources](#-additional-resources)
- [Contributing](#-contributing)

---

## 🎯 Introduction

This repository contains a curated collection of **57+ Linux interview questions** commonly asked in technical interviews for roles such as:

- ☁️ Cloud Engineers
- 🔧 DevOps Engineers
- 💻 System Administrators
- 🛠️ Site Reliability Engineers (SRE)
- 🔐 Security Engineers

Each section is organized by topic to help you prepare efficiently and systematically.

---

## 🔹 Basic Linux Concepts

**Common Questions:**

1. **What is Linux?**
2. **Difference between Linux and Unix**
3. **What is the Kernel?**
4. **What is Shell? Types of shells**
5. **What is root user?**
6. **What is the difference between CLI and GUI?**
7. **Explain Linux architecture**
8. **What is open-source?**

### 📖 Key Concepts to Understand:
- Linux kernel vs user space
- Linux distributions (Ubuntu, CentOS, RHEL, Debian)
- Unix philosophy and design principles
- Shell types: bash, sh, zsh, fish

---

## 🔹 File & Directory Management

**Common Questions:**

9. **Difference between file and directory**
10. **Explain Linux directory structure** (`/`, `/etc`, `/var`, `/home`, `/bin`)
11. **What does `ls -l` show?**
12. **Difference between `cp`, `mv`, and `rm`**
13. **What is inode?**
14. **Hard link vs Soft link**
15. **What is `find` command?**
16. **What is `grep`?**

### 📖 Key Concepts to Understand:
- Filesystem Hierarchy Standard (FHS)
- File metadata and attributes
- Search and filter commands
- Link types and use cases

---

## 🔹 Permissions & Ownership

**Common Questions:**

17. **What are r, w, x permissions?**
18. **Explain 755 / 777 permissions**
19. **Difference between chmod and chown**
20. **What is umask?**
21. **What is sticky bit?**
22. **What is SUID and SGID?**

### 📖 Key Concepts to Understand:
- Octal vs symbolic notation
- Special permissions (sticky bit, SUID, SGID)
- User, group, and other permissions
- Default permissions with umask

---

## 🔹 Process & System Management

**Common Questions:**

23. **What is a process?**
24. **Difference between process and thread**
25. **What is `ps`, `top`, `htop`?**
26. **How do you kill a process?**
27. **What is PID?**
28. **What is nice and renice?**
29. **What is load average?**
30. **How to check CPU and memory usage?**

### 📖 Key Concepts to Understand:
- Process lifecycle and states
- Process priority and scheduling
- System monitoring tools
- Signal handling (SIGTERM, SIGKILL)

---

## 🔹 Disk & File System

**Common Questions:**

31. **Difference between `df` and `du`**
32. **What is mounting?**
33. **What is `/etc/fstab`?**
34. **How to check disk space?**
35. **What is LVM?**
36. **Difference between ext4 and xfs**

### 📖 Key Concepts to Understand:
- Filesystem types and features
- Logical Volume Manager (LVM)
- Mount points and device mapping
- Disk usage analysis

---

## 🔹 Networking

**Common Questions:**

37. **How to check IP address in Linux?**
38. **Difference between `ifconfig` and `ip`**
39. **What is `netstat` / `ss`?**
40. **How to check open ports?**
41. **What is `ping` and `traceroute`?**
42. **How to test network connectivity?**

### 📖 Key Concepts to Understand:
- Network configuration commands
- TCP/IP fundamentals
- Port scanning and connectivity testing
- Legacy vs modern networking tools

---

## 🔹 Package Management

**Common Questions:**

43. **What is a package manager?**
44. **Difference between `yum`, `dnf`, `apt`**
45. **How to install/remove a package?**
46. **What is a repository?**
47. **Difference between rpm and deb?**

### 📖 Key Concepts to Understand:
- Debian-based vs RedHat-based distributions
- Package dependencies
- Repository management
- Package formats and compatibility

---

## 🔹 Logs & Troubleshooting

**Common Questions:**

48. **Where are Linux logs stored?**
49. **What is `/var/log`?**
50. **What is `journalctl`?**
51. **How to troubleshoot a Linux server?**
52. **What is `dmesg`?**

### 📖 Key Concepts to Understand:
- System logging (syslog, journald)
- Common log files and their purposes
- Troubleshooting methodology
- Kernel ring buffer

---

## 🔹 Shell Scripting

**Common Questions:**

53. **What is shell scripting?**
54. **Difference between bash and sh**
55. **What is a cron job?**
56. **How to schedule a task?**
57. **What are environment variables?**

### 📖 Key Concepts to Understand:
- Bash scripting basics
- Cron syntax and scheduling
- Environment vs shell variables
- Script automation

---

## 🔥 Top 10 Most Asked Questions

If you're short on time, focus on these critical questions:

1. ✅ **What is Linux and why is it used?**
2. ✅ **Difference between Linux and Unix**
3. ✅ **What is a process? How to kill it?**
4. ✅ **Explain Linux file permissions**
5. ✅ **Difference between `df` and `du`**
6. ✅ **What is `/etc/passwd` and `/etc/shadow`?**
7. ✅ **What is a cron job?**
8. ✅ **How to check memory and CPU usage?**
9. ✅ **What is a package manager?**
10. ✅ **How do you troubleshoot a Linux server?**

---

## 📖 Additional Resources

### Recommended Study Materials:
- 📘 [The Linux Command Line by William Shotts](http://linuxcommand.org/tlcl.php)
- 📘 [Linux Bible by Christopher Negus](https://www.wiley.com/en-us/Linux+Bible%2C+10th+Edition-p-9781119578895)
- 🎥 [Linux Foundation Training](https://training.linuxfoundation.org/)
- 🌐 [Linux Documentation Project](https://tldp.org/)

### Practice Platforms:
- 🖥️ [OverTheWire - Bandit](https://overthewire.org/wargames/bandit/)
- 🖥️ [Hack The Box](https://www.hackthebox.com/)
- 🖥️ [LeetCode System Design](https://leetcode.com/)

### Useful Commands Cheat Sheets:
- [Linux Command Cheat Sheet](https://www.linuxtrainingacademy.com/linux-commands-cheat-sheet/)
- [Vim Cheat Sheet](https://vim.rtorr.com/)

---

## 🤝 Contributing

Contributions are welcome! If you have additional questions or improvements:

1. Fork this repository
2. Create a new branch (`git checkout -b feature/new-questions`)
3. Commit your changes (`git commit -m 'Add new questions'`)
4. Push to the branch (`git push origin feature/new-questions`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## ⭐ Support

If you found this helpful, please give it a ⭐ and share it with others preparing for Linux interviews!

---

## 📧 Contact

For questions or suggestions, feel free to:
- Open an issue
- Submit a pull request
- Connect on [LinkedIn](https://linkedin.com/in/techwithburhan)

---

**Good luck with your interviews! 🚀**

*Last Updated: January 2026*

---

## Additional Resources

- Man pages: `man useradd`, `man chmod`, `man passwd`
- RedHat Documentation: https://access.redhat.com/documentation/
- Rocky Linux Wiki: https://wiki.rockylinux.org/

---
