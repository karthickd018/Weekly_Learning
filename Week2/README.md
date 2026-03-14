# Linux Filesystem Hierarchy

| Directory | Purpose                |
| --------- | ---------------------- |
| /         | Root directory         |
| /bin      | Essential commands     |
| /boot     | Boot files             |
| /dev      | Device files           |
| /etc      | Configuration files    |
| /home     | User directories       |
| /lib      | Libraries              |
| /media    | Removable media        |
| /mnt      | Mount point            |
| /opt      | Optional software      |
| /proc     | Process information    |
| /root     | Root user home         |
| /sbin     | System commands        |
| /tmp      | Temporary files        |
| /usr      | User applications      |
| /var      | Logs and variable data |



# Linux User Administration and Permissions

## User Administration Basics

A **user** is an account that can access and interact with the Linux system.

### Important Basic Commands

```bash
sudo useradd john        # Add a new user
sudo passwd john         # Set password for the user

sudo useradd -u 1100 smith   # Manually set user ID (UID)

cat /etc/passwd          # Displays username, UID, GID, home directory and default shell

ls -l /home/             # Display users home directories
ls -ln /home/            # Display user information in numeric format

id                       # Display UID, GID and group information
whoami                   # Display current logged-in username

useradd --help           # Display help information for useradd

sudo userdel username    # Delete user
sudo userdel -r john     # Delete user and remove home directory
```

### Switch User

```bash
su username
```

---

# Modify User Details

The **usermod** command is used to modify user account details.

### Change Home Directory

```bash
sudo usermod --home /home/otherdirectory --move-home john
```

or

```bash
sudo usermod -d /home/otherdirectory -m john
```

### Change Username

```bash
sudo usermod --login jane john
```

or

```bash
sudo usermod -l jane john
```

### Change Login Shell

```bash
sudo usermod --shell /bin/othershell john
```

or

```bash
sudo usermod -s /bin/othershell john
```

---

# Lock and Unlock User Account

```bash
sudo passwd -l username   # Lock user account
sudo passwd -u username   # Unlock user account
```

---

# Password Expiration

The **chage** command is used to manage password expiration policies.

| Option        | Description                         |
| ------------- | ----------------------------------- |
| `-d 0`        | Force password change on next login |
| `-d -1`       | Remove password expiration          |
| `-M 30`       | Password expires every 30 days      |
| `-M -1`       | Password never expires              |
| `-l username` | List password expiration details    |

Example:

```bash
sudo chage -d 0 jane
```

This means the user **jane must change the password on the next login**.

To view password aging information:

```bash
sudo chage -l jane
```

---

# Delete Users and Groups

```bash
sudo userdel -r jane   # Delete user and home directory
sudo groupdel john     # Delete group
```

---

# Local Groups and Membership

Groups help organize users and manage permissions efficiently.

### Create a Group

```bash
sudo groupadd developers
```

### Add User to Group

```bash
sudo gpasswd -a john developers
```

### Check Group Membership

```bash
groups john
```

### Remove User from Group

```bash
sudo gpasswd -d john developers
```

---

# Primary Group vs Secondary Group

When a user is created:

* A **primary group** is automatically created with the same name as the user.
* Additional groups are called **secondary groups**.

Example:

User: `john`
Primary Group: `john`
Secondary Group: `developers`

### Change Primary Group

```bash
sudo usermod -g developers john
```

Note:
When changing primary group, **group name comes first, then username**.

---

# Rename a Group

```bash
sudo groupmod -n newname oldname
```

Example:

```bash
sudo groupmod -n devteam developers
```

---

# Delete a Group

```bash
sudo groupdel groupname
```

Note:
You **cannot delete a group if it is the primary group of a user**.
First change the user's primary group before deleting.

---

# Linux File Permissions

Permissions control **who can read, write, or execute a file or directory**.

| Permission | Symbol | Meaning                   |
| ---------- | ------ | ------------------------- |
| Read       | r      | View file contents        |
| Write      | w      | Modify file               |
| Execute    | x      | Run the file as a program |

---

# Permission Categories

Linux divides users into three categories.

| User Type  | Meaning                 |
| ---------- | ----------------------- |
| User (u)   | File owner              |
| Group (g)  | Users in the same group |
| Others (o) | Everyone else           |

Example permission format:

```
rwx   r-x   r-x
 |     |     |
user group others
```

| Section | Meaning                          |
| ------- | -------------------------------- |
| rwx     | User can read, write and execute |
| r-x     | Group can read and execute       |
| r-x     | Others can read and execute      |

---

# Check File Permissions

To check permissions of files and directories:

```bash
ls -l
```

Check a specific file:

```bash
ls -l file.txt
```

Check a directory:

```bash
ls -ld foldername
```

---

# Create Files and Directories

```bash
touch file.txt      # Create a file
mkdir project       # Create a directory
```

Verify permissions:

```bash
ls -l
```

Example output:

```
-rw-r--r--  1 karthick  staff  200 Mar 12 10:20 file.txt
```

Explanation:

| Part | Meaning            |
| ---- | ------------------ |
| -    | File type          |
| rw-  | User permissions   |
| r--  | Group permissions  |
| r--  | Others permissions |


# Linux Process Management

## 1. Introduction

A **process** in Linux is a program that is currently running.
Whenever a command or application is executed, the operating system creates a process to perform that task.

Each process has a unique **Process ID (PID)** and uses system resources such as CPU and memory.

Example:

```bash
sleep 10
```

---

## 2. Process Details

Each process contains important information.

| Field | Description                       |
| ----- | --------------------------------- |
| PID   | Unique Process ID                 |
| PPID  | Parent Process ID                 |
| USER  | User who started the process      |
| CPU   | CPU usage                         |
| MEM   | Memory usage                      |
| CMD   | Command used to start the process |

Commands to view processes:

```bash
ps
```

Detailed view:

```bash
ps aux
```

Live monitoring:

```bash
top
```

---

## 3. Process Creation

A process is created whenever a user runs a command.

Example:

```bash
ls
```

Steps involved:

1. User enters a command in the shell.
2. The shell creates a new process.
3. The process executes the command.

To view process hierarchy:

```bash
pstree
```

---

## 4. Process States

Processes can exist in different states.

| State | Description |
| ----- | ----------- |
| R     | Running     |
| S     | Sleeping    |
| T     | Stopped     |
| Z     | Zombie      |

Check process states:

```bash
ps aux
```

---

## 5. Process Termination

### Normal Termination

A process ends automatically when it completes execution.

Example:

```bash
sleep 5
```

### Manual Termination

Processes can be stopped using the `kill` command.

```bash
kill PID
```

Example:

```bash
kill 1234
```

Force termination:

```bash
kill -9 1234
```

---

## 6. Signals in Linux

Signals are messages sent to processes to control their behavior.

Common signals:

| Signal  | Description                  |
| ------- | ---------------------------- |
| SIGINT  | Interrupt process (Ctrl + C) |
| SIGTERM | Gracefully terminate process |
| SIGKILL | Forcefully terminate process |
| SIGSTOP | Pause a process              |
| SIGCONT | Resume a process             |

List all signals:

```bash
kill -l
```

---

## 7. Job Control

Run a process in the background:

```bash
sleep 100 &
```

View background jobs:

```bash
jobs
```

Bring job to foreground:

```bash
fg
```

Pause a process:

Press:

```
CTRL + Z
```

Resume process in background:

```bash
bg
```

---

## 8. Important Commands Summary

| Command | Description                         |
| ------- | ----------------------------------- |
| ps      | Display running processes           |
| top     | Real-time process monitoring        |
| kill    | Send signals to terminate processes |
| pstree  | Show process hierarchy              |
| jobs    | List background jobs                |
| fg      | Bring job to foreground             |
| bg      | Resume job in background            |

| Signal  | Number | Description                  |
| ------- | ------ | ---------------------------- |
| SIGINT  | 2      | Interrupt process (Ctrl + C) |
| SIGQUIT | 3      | Quit process                 |
| SIGKILL | 9      | Force kill process           |
| SIGTERM | 15     | Graceful termination         |
| SIGSTOP | 19     | Stop process                 |
| SIGCONT | 18     | Continue stopped process     |


---

# INIT SYSTEM

what it is :
if we turn on a computer,linux kernel loads first 
after that it need a program to start other processes and services 
`that program is called init system`

Init system = the first process that starts after the Linux kernel boots.

	It manages:

	Starting services

	Stopping services

	System shutdown

	System reboot

## Boot Process Overview

1️⃣ BIOS / UEFI
Hardware initialization

2️⃣ Bootloader (GRUB)
Loads the Linux kernel

3️⃣ Kernel
Loads drivers and mounts root filesystem

4️⃣ Init system (PID 1)
Starts all system services

5️⃣ System ready	

## Responsibilities of Init System

The init system is responsible for:
| Task                     | Explanation             |
| ------------------------ | ----------------------- |
| Start services           | ssh, network, cron      |
| Stop services            | when shutting down      |
| Manage processes         | keep services running   |
| Handle runlevels/targets | system modes            |
| Boot initialization      | start system components |

## Types of Init Systems

There are three major init systems in Linux history.

| Init System | Description            |
| ----------- | ---------------------- |
| SysV Init   | Traditional Linux init |
| Upstart     | Event-based init       |
| systemd     | Modern Linux init      |

## SysV Init (System V Init)

This is the oldest init system.

It uses runlevels to control system states.

| Runlevel | Meaning          |
| -------- | ---------------- |
| 0        | Shutdown         |
| 1        | Single user mode |
| 3        | Multi-user mode  |
| 5        | Graphical mode   |
| 6        | Reboot           |

ex:init 0 - cmd used shutdown the system

## Upstart

Upstart was introduced to replace SysV Init.

It is `event-based`, 

`meaning services start when events happen`

Example events:

filesystem mounted

network started

hardware detected

## systemd (Modern Init System)

Most modern Linux distributions use systemd.

Examples:

Ubuntu

Debian

Fedora

CentOS

Arch

systemd is:

Faster

Parallel service startup

Better service management

## systemd Components

| Component  | Purpose               |
| ---------- | --------------------- |
| systemctl  | control services      |
| journald   | logging               |
| unit files | service configuration |
| targets    | replace runlevels     |


## systemd Targets (Runlevels Replacement)

| SysV Runlevel | systemd Target    |
| ------------- | ----------------- |
| 0             | poweroff.target   |
| 1             | rescue.target     |
| 3             | multi-user.target |
| 5             | graphical.target  |
| 6             | reboot.target     |


`An init system is the first process started by the Linux kernel during boot. It has PID 1 and is responsible for initializing the system and managing services and processes.`

## example init commands
|command|purpose|
|:---|:---|
|ps -p 1 | to check which init system my system uses|
|systemctl list-units --type=service | to check all running services|
|systemctl status ssh|to see the status of secure shell , u'll see service state ,logs,PID,
this helps to understand how init manages services|
|sudo systemctl start ssh| start a servie|
|sudo systemctl stop ssh|to stop a service|
|sudo systemctl stop ssh|Stop a Service|
|sudo systemctl restart ssh|restart a service|
|sudo systemctl enable ssh|enables ssh|
|systemctl is-enabled ssh| enabled or disabled|
|sudo systemctl disable ssh| to disable the ssh|
|systemd-analyze|this shows boot performance|

most important cmds:
	ps -p 1
	systemctl status ssh
	systemctl start ssh
	systemctl stop ssh
	systemctl enable ssh