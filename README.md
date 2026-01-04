# Linux User Management - Complete Guide & Hands-On Labs

## Table of Contents
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

---

## Additional Resources

- Man pages: `man useradd`, `man chmod`, `man passwd`
- RedHat Documentation: https://access.redhat.com/documentation/
- Rocky Linux Wiki: https://wiki.rockylinux.org/

---

**End of User Management Guide**
