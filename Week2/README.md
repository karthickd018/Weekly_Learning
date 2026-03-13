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

---

