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

# 🐳 Complete Docker Mastery Guide: Zero to Job-Ready

## 📋 Table of Contents & Learning Roadmap

This guide follows a **progressive learning path**:
- **Phase 1: Foundations** (Week 1-2)
- **Phase 2: Core Docker Skills** (Week 3-4)
- **Phase 3: Advanced Concepts** (Week 5-6)
- **Phase 4: Production & DevOps** (Week 7-8)

---

## PHASE 1: FOUNDATIONS

### 🔹 1. What is Docker and Why It Is Used

**Simple Explanation:**
Imagine you built a website on your laptop. It works perfectly. But when you try to run it on a server, it breaks because:
- Different operating system
- Missing libraries
- Wrong software versions

Docker solves this by packaging your application + all its dependencies into a **container** that runs the same way everywhere.

**Professional Depth:**

Docker is a **containerization platform** that allows you to:
1. **Package** applications with all dependencies
2. **Ship** them as lightweight, portable units
3. **Run** them consistently across any environment

**Why Docker is Used:**

1. **Consistency**: "It works on my machine" problem solved
2. **Isolation**: Applications don't interfere with each other
3. **Speed**: Start applications in seconds (vs minutes for VMs)
4. **Efficiency**: Share OS kernel, use fewer resources
5. **Scalability**: Easy to replicate and scale
6. **DevOps Integration**: Perfect for CI/CD pipelines

**Real-World Use Cases:**
- Microservices architecture
- CI/CD pipelines
- Local development environments
- Cloud deployments
- Testing different software versions

---

### 🔹 2. Virtual Machines vs Containers

**Simple Explanation:**

**Virtual Machine (VM):**
- Like having multiple complete computers inside one computer
- Each has its own operating system
- Heavy and slow to start

**Container:**
- Like having multiple apartments in one building
- All share the same foundation (OS kernel)
- Lightweight and fast to start

**Professional Depth:**

| Aspect | Virtual Machine | Container |
|--------|----------------|-----------|
| **OS** | Full OS per VM | Shares host OS kernel |
| **Size** | GBs (gigabytes) | MBs (megabytes) |
| **Startup** | Minutes | Seconds |
| **Resource Usage** | Heavy | Lightweight |
| **Isolation** | Complete | Process-level |
| **Portability** | Limited | Highly portable |

**Architecture Comparison:**

```
VIRTUAL MACHINE:
┌─────────────────────────────────┐
│     App A   │    App B          │
│  ┌────────┐ │ ┌────────┐        │
│  │ Bins/  │ │ │ Bins/  │        │
│  │ Libs   │ │ │ Libs   │        │
│  └────────┘ │ └────────┘        │
│  Guest OS   │  Guest OS         │
├─────────────┴──────────────────┤
│        Hypervisor               │
├─────────────────────────────────┤
│        Host OS                  │
├─────────────────────────────────┤
│        Hardware                 │
└─────────────────────────────────┘

CONTAINER:
┌─────────────────────────────────┐
│  App A   │   App B   │  App C   │
│ ┌──────┐ │ ┌──────┐  │ ┌──────┐│
│ │Bins/ │ │ │Bins/ │  │ │Bins/ ││
│ │Libs  │ │ │Libs  │  │ │Libs  ││
│ └──────┘ │ └──────┘  │ └──────┘│
├──────────┴───────────┴─────────┤
│      Docker Engine              │
├─────────────────────────────────┤
│        Host OS                  │
├─────────────────────────────────┤
│        Hardware                 │
└─────────────────────────────────┘
```

**When to Use What:**
- **Use VMs**: When you need complete OS isolation, different OS types, or strong security boundaries
- **Use Containers**: For microservices, DevOps, cloud-native apps, rapid scaling

---

### 🔹 3. Docker Architecture

**Simple Explanation:**
Docker works like a restaurant:
- **Docker Client** = Customer (you give orders)
- **Docker Daemon** = Kitchen (does the cooking)
- **Docker Registry** = Grocery Store (where ingredients come from)

**Professional Depth:**

Docker uses a **client-server architecture**:

```
┌──────────────┐
│ Docker Client│ (CLI: docker run, docker build, etc.)
└──────┬───────┘
       │ REST API
       ↓
┌──────────────────────────────────────┐
│     DOCKER DAEMON (dockerd)          │
│                                      │
│  ┌────────────────────────────────┐ │
│  │   Container Management         │ │
│  │   Image Management             │ │
│  │   Network Management           │ │
│  │   Volume Management            │ │
│  └────────────────────────────────┘ │
└──────────────┬───────────────────────┘
               │
               ↓
       ┌───────────────┐
       │ Docker Registry│ (Docker Hub, private registry)
       └───────────────┘
```

**Key Components:**

**1. Docker Client (`docker` CLI)**
- Interface you interact with
- Sends commands to Docker Daemon
- Can connect to remote daemons

**2. Docker Daemon (`dockerd`)**
- Background service running on host
- Manages Docker objects (images, containers, networks, volumes)
- Listens for Docker API requests
- Can communicate with other daemons

**3. Docker Registry**
- Stores Docker images
- **Docker Hub**: Public registry (default)
- **Private registries**: AWS ECR, Google GCR, Azure ACR, self-hosted

**4. Docker Objects:**
- **Images**: Read-only templates
- **Containers**: Runnable instances of images
- **Networks**: Communication between containers
- **Volumes**: Persistent data storage

**How It Works:**
```bash
$ docker run nginx

1. Docker Client sends command to Docker Daemon
2. Daemon checks if 'nginx' image exists locally
3. If not, pulls from Docker Hub
4. Creates a container from the image
5. Starts the container
```

---

### 🔹 4. Docker Installation (Linux-based)

**For Ubuntu/Debian:**

```bash
# 1. Update package index
sudo apt-get update

# 2. Install prerequisites
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# 3. Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 4. Set up stable repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. Install Docker Engine
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# 6. Verify installation
sudo docker run hello-world

# 7. Add your user to docker group (to run without sudo)
sudo usermod -aG docker $USER

# 8. Log out and log back in, then test
docker run hello-world
```

**For CentOS/RHEL:**

```bash
# 1. Remove old versions
sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine

# 2. Set up repository
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 3. Install Docker Engine
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# 4. Start Docker
sudo systemctl start docker
sudo systemctl enable docker

# 5. Verify
sudo docker run hello-world
```

**Post-Installation Verification:**

```bash
# Check Docker version
docker --version
docker version

# Check Docker info
docker info

# Check Docker Compose
docker compose version
```

---

## PHASE 2: CORE DOCKER SKILLS

### 🔹 5. Linux Users in Docker Context

**Simple Explanation:**
Just like your computer has user accounts (admin vs regular user), Docker containers also run with specific users for security.

**Professional Depth:**

**Linux User Types:**

1. **Root User (UID 0)**
   - Superuser with all privileges
   - Can do anything on the system
   - Security risk if compromised

2. **Normal User (UID ≥ 1000)**
   - Your regular user account
   - Limited privileges
   - Safer for everyday tasks

3. **System User (UID 1-999)**
   - Used by services/daemons
   - Examples: `www-data`, `mysql`, `nginx`
   - No login shell typically

**Docker User & Group:**

```bash
# Docker group created during installation
getent group docker
# Output: docker:x:999:your-username

# Why add user to docker group?
# Without it, you need sudo for every docker command
sudo docker ps    # Without group membership
docker ps         # With group membership
```

**Users in Docker Images:**

Every Docker image has a default user (usually root). You can check:

```bash
# Check user in an image
docker run --rm nginx whoami
# Output: root

docker run --rm node:alpine whoami
# Output: node (non-root user)
```

**Why Docker Uses Users in Images:**

1. **Security**: Limit what container processes can do
2. **Principle of Least Privilege**: Only give necessary permissions
3. **Host Protection**: If container is compromised, attacker has limited access

**Security Best Practices:**

**❌ Bad Practice (Running as Root):**
```dockerfile
FROM ubuntu:22.04
# Runs as root by default
COPY app.js /app/
CMD ["node", "/app/app.js"]
```

**✅ Good Practice (Non-Root User):**
```dockerfile
FROM ubuntu:22.04

# Create a non-root user
RUN groupadd -r appuser && useradd -r -g appuser appuser

# Set working directory
WORKDIR /app

# Copy files
COPY app.js /app/

# Change ownership
RUN chown -R appuser:appuser /app

# Switch to non-root user
USER appuser

CMD ["node", "app.js"]
```

**Running Containers as Non-Root:**

```bash
# Method 1: Specify user at runtime
docker run --user 1000:1000 nginx

# Method 2: Use USER instruction in Dockerfile
# (shown above)

# Method 3: Check what user container is running as
docker exec <container-id> whoami
```

**Real-World Security Scenario:**

```dockerfile
# Example: Node.js application with security best practices
FROM node:18-alpine

# Create app directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies as root (needed for npm)
RUN npm ci --only=production

# Copy application files
COPY . .

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Change ownership of app files
RUN chown -R nodejs:nodejs /app

# Switch to non-root user
USER nodejs

# Expose port
EXPOSE 3000

CMD ["node", "server.js"]
```

**Common Security Mistakes:**

1. **Running as root unnecessarily**
   ```bash
   # Bad: Allows full system access
   docker run -d --privileged nginx
   ```

2. **Mounting sensitive host paths**
   ```bash
   # Dangerous: Exposes host filesystem
   docker run -v /:/host ubuntu
   ```

3. **Not using USER instruction**
   - Always specify a non-root user in production images

**Checking Container User:**

```bash
# See what user a container runs as
docker inspect <container-id> | grep -i user

# Execute command as specific user
docker exec -u root <container-id> whoami
```

---

### 🔹 6. Docker Images Deep Dive

**Simple Explanation:**
A Docker image is like a recipe + ingredients for your application. It contains:
- Operating system files
- Your application code
- Dependencies and libraries
- Configuration files

**Professional Depth:**

**What is a Docker Image?**

A Docker image is a **read-only template** containing:
1. Base OS (Ubuntu, Alpine, etc.)
2. Application runtime (Node.js, Python, Java)
3. Application code
4. Dependencies
5. Configuration
6. Metadata

**Image Layers and UnionFS:**

Docker images are built in **layers** using a **Union File System**:

```
┌─────────────────────────────┐
│  Layer 4: CMD ["npm start"] │  <-- Metadata layer
├─────────────────────────────┤
│  Layer 3: COPY . /app       │  <-- Your app code
├─────────────────────────────┤
│  Layer 2: RUN npm install   │  <-- Dependencies
├─────────────────────────────┤
│  Layer 1: FROM node:18      │  <-- Base image
└─────────────────────────────┘
```

**Key Concepts:**

1. **Each instruction = New layer**
2. **Layers are cached** (speeds up builds)
3. **Layers are shared** between images
4. **Read-only** once created
5. **Container = Image + Writable layer**

**Example: How Layers Work**

```dockerfile
FROM ubuntu:22.04          # Layer 1 (Base OS)
RUN apt-get update         # Layer 2 (Update packages)
RUN apt-get install -y nginx  # Layer 3 (Install nginx)
COPY index.html /var/www/html/  # Layer 4 (Copy file)
CMD ["nginx", "-g", "daemon off;"]  # Layer 5 (Start command)
```

Each `RUN`, `COPY`, `ADD` creates a new layer.

**View Image Layers:**

```bash
# See layers of an image
docker history nginx:alpine

# Output shows:
# IMAGE          CREATED       CREATED BY                                      SIZE
# abc123         2 weeks ago   CMD ["nginx" "-g" "daemon off;"]               0B
# def456         2 weeks ago   COPY index.html /usr/share/nginx/html/         4kB
# ghi789         2 weeks ago   RUN apk add --no-cache nginx                   50MB
# ...
```

**Docker Hub & Registries:**

```bash
# Default registry: Docker Hub (hub.docker.com)

# Pull an image
docker pull nginx
docker pull nginx:1.25-alpine  # Specific version

# Pull from private registry
docker pull myregistry.com/myapp:v1.0

# Search for images
docker search nginx

# Official vs Community images
# Official: nginx, ubuntu, node (verified by Docker)
# Community: username/imagename
```

**Image Operations:**

```bash
# List all images
docker images
docker image ls

# Pull image from registry
docker pull ubuntu:22.04

# Tag an image (create alias)
docker tag nginx:latest myapp:v1.0
docker tag myapp:v1.0 myregistry.com/myapp:v1.0

# Push image to registry
docker login  # Login first
docker push myregistry.com/myapp:v1.0

# Remove image
docker rmi nginx:latest
docker image rm nginx:latest

# Remove multiple images
docker rmi $(docker images -q)
```

**Image Inspection:**

```bash
# Detailed information about an image
docker inspect nginx:alpine

# See specific fields
docker inspect nginx:alpine --format='{{.Architecture}}'
docker inspect nginx:alpine --format='{{.Config.Env}}'

# Check image size
docker images nginx:alpine --format "{{.Size}}"

# See image layers in detail
docker history nginx:alpine --no-trunc
```

**Dangling Images:**

**Simple Explanation:**
Dangling images are "orphaned" images that have no tag and are not used by any container.

```bash
# How dangling images are created:
docker build -t myapp:v1 .
docker build -t myapp:v1 .  # Rebuild with same tag
# The old image becomes dangling (tag moved to new image)

# List dangling images
docker images -f "dangling=true"

# Remove dangling images
docker image prune

# Remove all unused images
docker image prune -a
```

**Image Naming Convention:**

```
[registry-url]/[namespace]/[repository]:[tag]

Examples:
nginx                          # Official image
nginx:1.25                     # Specific version
nginx:1.25-alpine             # Variant
docker.io/library/nginx:latest # Full format
myregistry.com/team/app:v1.0  # Private registry
```

**Best Practices:**

1. **Always use specific tags** (not `latest`)
   ```bash
   # Bad
   FROM node:latest
   
   # Good
   FROM node:18.17.1-alpine
   ```

2. **Use official images** when possible

3. **Keep images small** (prefer Alpine variants)

4. **Clean up regularly**
   ```bash
   docker image prune -a --filter "until=24h"
   ```

---

### 🔹 7. Docker Containers Deep Dive

**Simple Explanation:**
If an image is a recipe, a container is the actual dish you cook. It's a **running instance** of an image.

**Professional Depth:**

**Container Lifecycle:**

```
        docker create
             ↓
        ┌─────────┐
        │ CREATED │
        └────┬────┘
             │ docker start
             ↓
        ┌─────────┐     docker pause
        │ RUNNING │ ←─────────────┐
        └────┬────┘                │
             │                     │
             │ docker pause   ┌────┴────┐
             └───────────────→│ PAUSED  │
             │                └─────────┘
             │ docker stop          ↑
             ↓                      │
        ┌─────────┐                 │
        │ STOPPED │─────────────────┘
        └────┬────┘     docker unpause
             │
             │ docker rm
             ↓
        ┌─────────┐
        │ REMOVED │
        └─────────┘
```

**Running Containers:**

**Foreground vs Background:**

```bash
# Foreground (attached) - blocks terminal
docker run nginx
# You see logs, Ctrl+C stops container

# Background (detached) - runs in background
docker run -d nginx
# Returns container ID, terminal is free

# Run and get interactive shell
docker run -it ubuntu bash
# -i = interactive (keep STDIN open)
# -t = tty (allocate pseudo-terminal)

# Run in background with name
docker run -d --name my-nginx nginx
```

**Container Naming:**

```bash
# Docker auto-generates names if not specified
docker run -d nginx
# Creates name like "quirky_einstein"

# Better: specify name
docker run -d --name web-server nginx

# Name must be unique
docker run -d --name web-server nginx  # Error: name already in use

# Remove and recreate
docker rm -f web-server
docker run -d --name web-server nginx
```

**Restart Policies:**

```bash
# no: Don't restart (default)
docker run -d --restart=no nginx

# on-failure: Restart if exits with error
docker run -d --restart=on-failure nginx
docker run -d --restart=on-failure:5 nginx  # Max 5 retries

# always: Always restart (even after reboot)
docker run -d --restart=always nginx

# unless-stopped: Like always, but respect manual stop
docker run -d --restart=unless-stopped nginx

# Update restart policy of running container
docker update --restart=always my-container
```

**Real-World Example:**
```bash
# Production web server with auto-restart
docker run -d \
  --name production-nginx \
  --restart=unless-stopped \
  -p 80:80 \
  nginx:1.25-alpine
```

**Resource Limits:**

```bash
# Limit memory
docker run -d --memory="512m" nginx
docker run -d --memory="512m" --memory-swap="1g" nginx

# Limit CPU
docker run -d --cpus="1.5" nginx  # 1.5 CPU cores
docker run -d --cpu-shares=512 nginx  # Relative weight

# Limit both
docker run -d \
  --name resource-limited-app \
  --memory="1g" \
  --cpus="2" \
  --memory-swap="2g" \
  my-app:latest

# Check resource usage
docker stats
docker stats my-container

# Real-time resource monitoring
docker stats --no-stream
```

**Container Logs:**

```bash
# View logs
docker logs my-container

# Follow logs (like tail -f)
docker logs -f my-container

# Last N lines
docker logs --tail 100 my-container

# Logs with timestamps
docker logs -t my-container

# Logs since specific time
docker logs --since 2024-01-01 my-container
docker logs --since 10m my-container  # Last 10 minutes

# Combination
docker logs -f --tail 50 --since 5m my-container
```

**Exec vs Attach:**

**docker exec** - Run NEW command in running container:
```bash
# Execute command
docker exec my-container ls /app

# Get interactive shell
docker exec -it my-container bash
docker exec -it my-container sh  # For Alpine

# Run as specific user
docker exec -u root my-container whoami

# Execute in specific directory
docker exec -w /app my-container pwd
```

**docker attach** - Attach to RUNNING process (PID 1):
```bash
# Attach to main process
docker attach my-container
# You see output of main process
# Ctrl+C will STOP the container!

# Attach without stopping (use Ctrl+P then Ctrl+Q to detach)
docker attach --sig-proxy=false my-container
```

**When to use what:**
- **exec**: Debug, run commands, get shell (most common)
- **attach**: View main process output (rarely used)

**Container Management:**

```bash
# List running containers
docker ps
docker container ls

# List all containers (including stopped)
docker ps -a
docker container ls -a

# Pause container (freeze processes)
docker pause my-container
docker unpause my-container

# Stop container (graceful shutdown, 10s timeout)
docker stop my-container
docker stop -t 30 my-container  # 30s timeout

# Kill container (force kill immediately)
docker kill my-container

# Restart container
docker restart my-container

# Remove container
docker rm my-container

# Remove running container (force)
docker rm -f my-container

# Remove all stopped containers
docker container prune

# Remove all containers (dangerous!)
docker rm -f $(docker ps -aq)
```

**Container Inspection:**

```bash
# Full details
docker inspect my-container

# Specific field
docker inspect my-container --format='{{.State.Status}}'
docker inspect my-container --format='{{.NetworkSettings.IPAddress}}'

# Container process information
docker top my-container

# Container resource stats
docker stats my-container

# Container port mappings
docker port my-container

# Container filesystem changes
docker diff my-container
```

**Practical Examples:**

**Example 1: Web Server with Proper Configuration**
```bash
docker run -d \
  --name production-web \
  --restart=unless-stopped \
  --memory="1g" \
  --cpus="2" \
  -p 80:80 \
  -p 443:443 \
  -v /var/www/html:/usr/share/nginx/html:ro \
  -v /etc/nginx/conf.d:/etc/nginx/conf.d:ro \
  nginx:1.25-alpine
```

**Example 2: Database with Persistence**
```bash
docker run -d \
  --name postgres-db \
  --restart=unless-stopped \
  --memory="2g" \
  -e POSTGRES_PASSWORD=secretpass \
  -e POSTGRES_USER=admin \
  -e POSTGRES_DB=myapp \
  -v postgres-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15-alpine
```

**Example 3: Development Environment**
```bash
docker run -it --rm \
  --name dev-env \
  -v $(pwd):/workspace \
  -w /workspace \
  -p 3000:3000 \
  node:18-alpine \
  sh
```

---

### 🔹 8. Dockerfile Mastery (Very Important)

**Simple Explanation:**
A Dockerfile is a recipe for building Docker images. It's a text file with instructions telling Docker how to build your image.

**Professional Depth:**

**Dockerfile Structure:**

```dockerfile
# Comments start with #
# Instructions are UPPERCASE (convention)
# Each instruction creates a layer

# Syntax:
INSTRUCTION arguments
```

**Core Instructions:**

**1. FROM - Base Image**

```dockerfile
# Always first instruction (except ARG)
FROM ubuntu:22.04

# Multi-platform
FROM --platform=linux/amd64 ubuntu:22.04

# From scratch (empty image)
FROM scratch

# Multiple FROM (multi-stage builds)
FROM node:18 AS builder
FROM nginx:alpine AS runner
```

**2. RUN - Execute Commands**

```dockerfile
# Shell form (runs in /bin/sh -c)
RUN apt-get update && apt-get install -y nginx

# Exec form (doesn't invoke shell)
RUN ["apt-get", "update"]

# Multiple commands (creates multiple layers - BAD)
RUN apt-get update
RUN apt-get install -y nginx
RUN apt-get clean

# Better: Chain commands (single layer)
RUN apt-get update && \
    apt-get install -y nginx && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Alpine example
RUN apk add --no-cache \
    nginx \
    curl \
    && rm -rf /var/cache/apk/*
```

**3. CMD - Default Command**

```dockerfile
# Only one CMD per Dockerfile (last one wins)
# Can be overridden at runtime

# Shell form
CMD npm start

# Exec form (preferred)
CMD ["npm", "start"]

# With ENTRYPOINT
CMD ["--port", "3000"]
```

**4. ENTRYPOINT - Container Executable**

```dockerfile
# Makes container run as executable
# Cannot be overridden easily (need --entrypoint flag)

# Exec form
ENTRYPOINT ["nginx", "-g", "daemon off;"]

# With CMD (CMD becomes default arguments)
ENTRYPOINT ["node"]
CMD ["server.js"]

# Override at runtime:
# docker run myimage app.js  # Runs: node app.js
```

**CMD vs ENTRYPOINT Deep Dive:**

```dockerfile
# Example 1: CMD only
FROM ubuntu
CMD ["echo", "Hello"]
# docker run myimage           → Hello
# docker run myimage echo Bye  → Bye (CMD replaced)

# Example 2: ENTRYPOINT only
FROM ubuntu
ENTRYPOINT ["echo"]
# docker run myimage           → (no output)
# docker run myimage Hello     → Hello

# Example 3: Both (BEST PRACTICE)
FROM ubuntu
ENTRYPOINT ["echo"]
CMD ["Hello World"]
# docker run myimage           → Hello World
# docker run myimage Goodbye   → Goodbye
```

**5. COPY vs ADD (Critical Difference)**

**Simple Explanation:**
- **COPY**: Simple file copy (preferred)
- **ADD**: Copy + extra features (auto-extract tar, download URLs)

**COPY:**
```dockerfile
# Basic copy
COPY index.html /var/www/html/

# Copy with patterns
COPY *.js /app/

# Copy directory
COPY src/ /app/src/

# Multiple sources
COPY package*.json /app/

# Change ownership
COPY --chown=user:group src/ /app/

# From build stage (multi-stage)
COPY --from=builder /app/dist /usr/share/nginx/html/
```

**ADD:**
```dockerfile
# Same as COPY
ADD index.html /var/www/html/

# Auto-extract tar files (can be dangerous)
ADD archive.tar.gz /app/
# Automatically extracts to /app/

# Download from URL (NOT recommended - no checksum verification)
ADD https://example.com/file.tar.gz /tmp/
```

**When to Use What:**

✅ **Use COPY** (99% of the time):
- Copying local files/directories
- Predictable behavior
- Security best practice

❌ **Avoid ADD** unless:
- You specifically need tar extraction
- Even then, be explicit:
  ```dockerfile
  COPY archive.tar.gz /tmp/
  RUN tar -xzf /tmp/archive.tar.gz -C /app/ && rm /tmp/archive.tar.gz
  ```

**Why COPY is Better:**
1. **Explicit**: Does exactly what it says
2. **Predictable**: No magic behavior
3. **Secure**: Doesn't auto-download from internet
4. **Recommended**: Docker best practices

**6. ENV - Environment Variables**

```dockerfile
# Set environment variable
ENV NODE_ENV=production

# Multiple variables
ENV NODE_ENV=production \
    PORT=3000 \
    LOG_LEVEL=info

# Used in subsequent instructions
ENV APP_HOME=/app
WORKDIR $APP_HOME

# Override at runtime:
docker run -e NODE_ENV=development myimage
```

**7. ARG - Build Arguments**

```dockerfile
# Available only during build time
ARG VERSION=1.0
ARG NODE_VERSION=18

# Use in FROM
ARG NODE_VERSION=18
FROM node:${NODE_VERSION}

# Use in RUN
ARG BUILD_DATE
RUN echo "Built on ${BUILD_DATE}"

# Provide at build time:
docker build --build-arg NODE_VERSION=20 .

# ARG vs ENV:
# ARG: Build time only
# ENV: Build time + runtime
```

**8. WORKDIR - Working Directory**

```dockerfile
# Set working directory (creates if not exists)
WORKDIR /app

# All subsequent commands run from here
COPY . .
RUN npm install

# Use variables
ENV APP_DIR=/application
WORKDIR $APP_DIR

# Multiple WORKDIR (relative paths)
WORKDIR /app
WORKDIR subdir  # Now at /app/subdir
```

**9. EXPOSE - Document Ports**

```dockerfile
# Document which ports the container listens on
EXPOSE 80
EXPOSE 443
EXPOSE 8080/tcp
EXPOSE 8081/udp

# Multiple ports
EXPOSE 80 443 8080

# NOTE: EXPOSE doesn't publish ports!
# It's documentation only
# Still need: docker run -p 80:80 myimage
```

**10. USER - Switch User**

```dockerfile
# Switch to user (security best practice)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Everything after runs as appuser
RUN whoami  # Outputs: appuser

# Switch back to root
USER root
RUN apt-get update
USER appuser

# With UID:GID
USER 1000:1000
```

**11. VOLUME - Declare Volumes**

```dockerfile
# Declare volumes (creates mount point)
VOLUME /data

# Multiple volumes
VOLUME /data /logs

# NOTE: Better to define volumes at runtime
# docker run -v mydata:/data myimage
```

**12. LABEL - Metadata**

```dockerfile
# Add metadata to image
LABEL maintainer="your.email@example.
