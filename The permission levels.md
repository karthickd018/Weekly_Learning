# What are Permissions in Linux?

Permissions control **who can read, write, or execute a file or directory**.

|Permission|Symbol|Meaning|
|---|---|---|
|Read|r|View file content|
|Write|w|Modify the file|
|Execute|x|Run the file (script/program)|
# Who gets the Permissions?

Linux divides users into **3 categories**.

| User Type      | Meaning                 |
| -------------- | ----------------------- |
| **User (u)**   | File owner              |
| **Group (g)**  | Users in the same group |
| **Others (o)** | Everyone else           |

rwx       r-x         r-x
 |             |              |
user    group    others

| Section | Permission                  |
| ------- | --------------------------- |
| rwx     | user can read write execute |
| r-x     | group can read execute      |
| r-x     | others can read execute     |
## 3. Check Permission of a File

To check the permissions of files and directories in Linux we use the **`ls -l` command**.

check_permissions_current_directory = ls -l
check_permission_file = ls -l file.txt
check_permission_directory = ls -ld foldername
create_file = touch file.txt
create_directory = mkdir project
verify_permissions = ls -l

example output :   -rw-r--r--  1 karthick  staff  200 Mar 12 10:20 file.txt

explanation for the above example:

| Part  | Meaning            |
| ----- | ------------------ |
| `-`   | File type          |
| `rw-` | User permissions   |
| `r--` | Group permissions  |
| `r--` | Others permissions |
