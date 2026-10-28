# 🐧 Linux Commands Reference — Beginner to DevOps

A practical reference for learning Linux commands on Ubuntu, AWS EC2, and other Linux systems.

## Table of Contents

1. [Linux Basics](#1-linux-basics)
2. [Directory Navigation](#2-directory-navigation)
3. [Create Directories](#3-create-directories)
4. [Create and Manage Files](#4-create-and-manage-files)
5. [List Files and Directories](#5-list-files-and-directories)
6. [View File Contents — cat, head, tail, more, less](#6-view-file-contents)
7. [Copy, Move, Rename, and Delete](#7-copy-move-rename-and-delete)
8. [Text Output and Redirection](#8-text-output-and-redirection)
9. [Search Files and Text](#9-search-files-and-text)
10. [File Permissions and Ownership](#10-file-permissions-and-ownership)
11. [System Information](#11-system-information)
12. [Memory, CPU, and Disk Monitoring](#12-memory-cpu-and-disk-monitoring)
13. [Processes and Services](#13-processes-and-services)
14. [Networking Commands](#14-networking-commands)
15. [Package Management in Ubuntu](#15-package-management-in-ubuntu)
16. [Archives and Compression](#16-archives-and-compression)
17. [Users and Groups](#17-users-and-groups)
18. [Command History and Terminal Shortcuts](#18-command-history-and-terminal-shortcuts)
19. [Important Linux Operators](#19-important-linux-operators)
20. [Essential AWS EC2 Troubleshooting Commands](#20-essential-aws-ec2-troubleshooting-commands)
21. [Safe Hands-on Practice](#21-safe-hands-on-practice)
22. [Interview Questions](#22-interview-questions)
23. [Quick Revision Cheat Sheet](#23-quick-revision-cheat-sheet)

---

## 1. Linux Basics

### What is a Linux command?

A Linux command is an instruction given to the operating system through a terminal.

**General syntax:**

```bash
command [options] [arguments]
```

Example:

```bash
ls -lh /home/ubuntu
```

- `ls` — command to list files.
- `-lh` — options to display detailed information and human-readable sizes.
- `/home/ubuntu` — target directory.

### Show the current directory

```bash
pwd
```

Example output:

```text
/home/ubuntu
```

### Display the current username

```bash
whoami
```

### Show the current date and time

```bash
date
```

### Display Linux distribution information

```bash
cat /etc/os-release
```

### Show the Linux kernel version

```bash
uname -r
```

### Show complete kernel and system information

```bash
uname -a
```

### Display the hostname

```bash
hostname
```

---

## 2. Directory Navigation

| Command | Purpose |
|---|---|
| `pwd` | Show current directory |
| `cd /home/ubuntu` | Go to a specific directory |
| `cd ~` | Go to your home directory |
| `cd` | Usually returns to your home directory |
| `cd .` | Stay in the current directory |
| `cd ..` | Go up one directory |
| `cd ../..` | Go up two directory levels |
| `cd -` | Return to the previous working directory |
| `ls` | List directory contents |

### Example directory structure

```text
/home/ubuntu/
├── projects/
│   ├── linux/
│   └── python/
└── documents/
```

If your current directory is `/home/ubuntu/projects/linux`:

```bash
cd ..
```

You move to:

```text
/home/ubuntu/projects
```

**Memory trick:** `..` means parent directory; `.` means current directory; `~` means your home directory.

---

## 3. Create Directories

### Create one directory

```bash
mkdir devops
```

### Create multiple directories

```bash
mkdir aws docker kubernetes
```

### Create parent and child directories

```bash
mkdir -p parent/child
```

### Create a directory tree using brace expansion

```bash
mkdir -p project/{src,bin,docs}
```

This creates:

```text
project/
├── src/
├── bin/
└── docs/
```

### Display directories created

```bash
mkdir -v terraform
```

### Create a directory with specific permissions

```bash
mkdir -m 700 private
```

### View directory permissions

```bash
ls -ld private
```

**Remember:**

- `-p` = create missing parent directories.
- `-v` = verbose output.
- `-m` = permissions at creation time.

---

## 4. Create and Manage Files

### Create an empty file

```bash
touch file.txt
```

### Create multiple files

```bash
touch file1.txt file2.txt file3.txt
```

### Create a hidden file

```bash
touch .hiddenfile
```

Linux filenames beginning with `.` are hidden by default in ordinary `ls` output.

### Create files using brace expansion

```bash
touch file{1..5}.log
```

Creates `file1.log` through `file5.log`.

### Create a file using an editor

```bash
vim notes.txt
```

Or use Nano, if installed:

```bash
nano notes.txt
```

### Identify a file's type

```bash
file filename.txt
```

### Display file metadata

```bash
stat filename.txt
```

### Count lines, words, and bytes

```bash
wc filename.txt
```

Individual counts:

```bash
wc -l filename.txt
wc -w filename.txt
wc -c filename.txt
```

- `-l` = lines.
- `-w` = words.
- `-c` = bytes.

---

## 5. List Files and Directories

| Command | Purpose |
|---|---|
| `ls` | List visible entries |
| `ls -l` | Long listing with permissions, owner, size, and date |
| `ls -lh` | Long listing with human-readable sizes |
| `ls -a` | Include hidden entries |
| `ls -la` | Include hidden entries with details |
| `ls -ld folder` | Show details of the directory itself |
| `ls -R` | List directories recursively |
| `ls -lt` | Sort by modification time, newest first |
| `ls -ltr` | Sort by modification time, oldest first |
| `ls -lhS` | Sort by size, largest first |
| `ls -lhSr` | Sort by size, smallest first |

### Example

```bash
ls -lah
```

Displays visible and hidden entries, with detailed information and human-readable sizes.

**Tip:** In `ls -l`, the first character indicates the type: `d` for directory, `-` for a regular file, and `l` for a symbolic link.

---

## 6. View File Contents

This section is important for Linux administration, log analysis, and DevOps troubleshooting.

### `cat` — display a file

```bash
cat file.txt
```

Display line numbers:

```bash
cat -n file.txt
```

Combine multiple files for viewing:

```bash
cat file1.txt file2.txt
```

Best for short files. Large files can fill the terminal quickly.

### `head` — display the beginning of a file

```bash
head file.txt
```

By default, displays the first 10 lines.

Display the first five lines:

```bash
head -n 5 file.txt
```

Display the first 20 lines:

```bash
head -n 20 file.txt
```

### `tail` — display the end of a file

```bash
tail file.txt
```

By default, displays the last 10 lines.

Display the last five lines:

```bash
tail -n 5 file.txt
```

Display the last 50 lines:

```bash
tail -n 50 file.txt
```

### Follow a log file live

```bash
tail -f /var/log/syslog
```

Displays new lines as they are appended. Press `Ctrl + C` to stop following.

A useful option when a log file may be replaced or recreated:

```bash
tail -F /var/log/syslog
```

### `less` — browse a file page by page

```bash
less file.txt
```

Useful for large files and logs.

| Key | Action inside `less` |
|---|---|
| `Space` | Next page |
| `b` | Previous page |
| `/error` | Search forward for `error` |
| `?error` | Search backward for `error` |
| `n` | Next search match |
| `N` | Previous search match |
| `g` | Beginning of file |
| `G` | End of file |
| `q` | Quit |

### `more` — basic page-by-page viewing

```bash
more file.txt
```

- `Space` — next page.
- `Enter` — next line.
- `q` — quit.

`less` is generally more flexible because it supports backward navigation and searching.

### Quick comparison

| Command | Best use |
|---|---|
| `cat` | Short files |
| `head` | First lines |
| `tail` | Last lines |
| `tail -f` | Monitor live logs |
| `less` | Browse and search large files |
| `more` | Simple page-by-page viewing |

### Practical log examples

```bash
head -n 20 /var/log/syslog
tail -n 30 /var/log/syslog
less /var/log/syslog
```

On Ubuntu, some logs may require `sudo` to read, and not every log file exists on every installation.

---

## 7. Copy, Move, Rename, and Delete

### Copy a file

```bash
cp file.txt backup.txt
```

### Copy a file into a directory

```bash
cp file.txt /tmp/
```

### Copy a directory recursively

```bash
cp -r project project_backup
```

### Preserve file attributes while copying

```bash
cp -a project project_backup
```

### Move a file

```bash
mv file.txt /tmp/
```

### Rename a file

```bash
mv oldname.txt newname.txt
```

### Delete one file

```bash
rm file.txt
```

### Delete multiple files

```bash
rm file1.txt file2.txt
```

### Delete files matching a pattern

```bash
rm -- *.txt
```

This matches `.txt` files in the current directory, not files recursively in all subdirectories.

### Delete an empty directory

```bash
rmdir empty_folder
```

### Delete a directory and its contents

```bash
rm -r foldername
```

**Danger:** `rm -rf foldername` forcibly removes the named directory tree without prompting in normal use. A wrong path can cause data loss.

Before deleting, check the target:

```bash
pwd
ls -la foldername
```

### About keeping selected files

Avoid experimenting with commands such as:

```bash
rm -rf !(keepdir)
```

This uses Bash extended globbing, which must be enabled, and a pattern or directory mistake can delete unintended files. For important data, first list exactly what would be affected and use a backup or a dedicated temporary practice directory.

---

## 8. Text Output and Redirection

### Display a message

```bash
echo "Hello, Linux"
```

### Write text to a file

```bash
echo "Hello" > file.txt
```

`>` creates or overwrites the file.

### Append text to a file

```bash
echo "More text" >> file.txt
```

`>>` appends without replacing existing content.

### Save command output

```bash
ls -l > files.txt
```

### Append command output

```bash
ls -l >> files.txt
```

### Display output and save it simultaneously

```bash
ls -l | tee output.txt
```

Append instead of overwrite:

```bash
ls -l | tee -a output.txt
```

### Redirect errors

```bash
ls /not-existing 2> errors.txt
```

### Redirect normal output and errors

```bash
command > output.txt 2>&1
```

This saves standard output and standard error in the same file in the order shown.

### Discard output

```bash
command > /dev/null 2>&1
```

Use this only when you intentionally do not need the output or error messages.

---

## 9. Search Files and Text

### `grep` — search text

```bash
grep "error" file.txt
```

Ignore case:

```bash
grep -i "error" file.txt
```

Show matching line numbers:

```bash
grep -n "error" file.txt
```

Search recursively in a directory:

```bash
grep -r "error" ./logs/
```

Show lines that do not match:

```bash
grep -v "success" file.txt
```

Count matching lines:

```bash
grep -c "error" file.txt
```

### Search logs for errors

```bash
grep -i "error" /var/log/syslog
```

### `find` — search for files and directories

Find a file by name:

```bash
find . -name "file.txt"
```

Find all `.log` files:

```bash
find . -type f -name "*.log"
```

Find directories named `backup`:

```bash
find . -type d -name "backup"
```

Find files modified in the last day:

```bash
find . -type f -mtime -1
```

### `which` and `command -v` — find a command

```bash
which python3
```

A shell-friendly alternative:

```bash
command -v python3
```

### `whereis` — locate common files associated with a command

```bash
whereis python3
```

---

## 10. File Permissions and Ownership

Linux permissions control who can read, write, and execute a file or access a directory.

### View permissions

```bash
ls -l file.txt
```

Example:

```text
-rw-r--r-- 1 ubuntu ubuntu 100 Oct 28 10:00 file.txt
```

### Permission meanings

| Permission | File meaning | Directory meaning |
|---|---|---|
| `r` | Read contents | List names |
| `w` | Modify contents | Create or remove entries, subject to other permissions |
| `x` | Execute file | Traverse or access entries |

### Change permissions

```bash
chmod 644 file.txt
```

Give the owner execute permission:

```bash
chmod u+x script.sh
```

Make a script executable:

```bash
chmod +x script.sh
```

Set private directory permissions:

```bash
chmod 700 private
```

### Change ownership

```bash
sudo chown ubuntu file.txt
```

Change owner and group:

```bash
sudo chown ubuntu:ubuntu file.txt
```

Change ownership recursively only when you have verified the target:

```bash
sudo chown -R ubuntu:ubuntu /path/to/project
```

### Permission number memory trick

- `r = 4`
- `w = 2`
- `x = 1`

Therefore:

- `7 = rwx`
- `6 = rw-`
- `5 = r-x`
- `4 = r--`

Example: `755` means owner `rwx`, group `r-x`, and others `r-x`.

---

## 11. System Information

| Command | Purpose |
|---|---|
| `hostname` | Show system hostname |
| `hostnamectl` | Show or manage hostname information |
| `uname -a` | Kernel and system information |
| `uname -r` | Kernel version |
| `cat /etc/os-release` | Linux distribution details |
| `whoami` | Current username |
| `id` | User and group IDs |
| `who` | Users currently logged in |
| `w` | Logged-in users and activity |
| `uptime` | System uptime and load averages |
| `date` | Current date and time |
| `timedatectl` | Time and timezone information |
| `cal` | Display a calendar, if installed |
| `env` | Display environment variables |
| `printenv HOME` | Display the `HOME` environment variable |

---

## 12. Memory, CPU, and Disk Monitoring

### Memory usage

```bash
free -h
```

Displays total, used, available, and swap memory.

### Disk space

```bash
df -h
```

Shows filesystem capacity and available space.

Show filesystem type:

```bash
df -hT
```

### Directory size

```bash
du -sh /home/ubuntu
```

Show sizes of entries inside the current directory:

```bash
du -sh ./*
```

### CPU information

```bash
lscpu
```

### Process and resource monitoring

```bash
top
```

Exit `top` with `q`.

If installed, an interactive alternative is:

```bash
htop
```

### Load average

```bash
uptime
```

The three load averages represent approximate system load over 1, 5, and 15 minutes. They are **not CPU utilization percentages**.

### Block devices

```bash
lsblk
```

### Disk partitions and filesystem details

```bash
sudo fdisk -l
```

This lists partition information. Do not run disk-changing operations unless you understand the target device and the consequences.

### Check inode availability

```bash
df -i
```

Useful when a filesystem has free space but cannot create additional files because it has run out of inodes.

---

## 13. Processes and Services

### List running processes

```bash
ps aux
```

### Find a process by name

```bash
pgrep -a python
```

### Search process output

```bash
ps aux | grep python
```

### Stop a process by PID

```bash
kill PID
```

Replace `PID` with the actual process ID.

### Forcefully stop a process

```bash
kill -9 PID
```

Use this only when a normal termination signal does not work. The process may not get a chance to clean up.

### Check a systemd service

```bash
systemctl status cron
```

### Start, stop, or restart a service

```bash
sudo systemctl start cron
sudo systemctl stop cron
sudo systemctl restart cron
```

### Enable a service at boot

```bash
sudo systemctl enable cron
```

### View service logs

```bash
journalctl -u cron
```

View recent service logs:

```bash
journalctl -u cron -n 50
```

Follow logs live:

```bash
journalctl -u cron -f
```

---

## 14. Networking Commands

### Show IP addresses

```bash
ip addr
```

Show routing information:

```bash
ip route
```

Show the hostname:

```bash
hostname
```

Display assigned IP addresses in brief:

```bash
hostname -I
```

These are generally private or locally assigned addresses on an EC2 instance.

### Test connectivity

```bash
ping -c 4 8.8.8.8
```

Some environments block ICMP, so a failed ping does not always mean a service is unavailable.

### Make HTTP requests

```bash
curl -I https://example.com
```

Fetch a webpage's content:

```bash
curl https://example.com
```

### Check DNS resolution

```bash
getent hosts example.com
```

### Check listening ports

```bash
sudo ss -tulpn
```

### Test whether a TCP port is reachable

If `nc` is installed:

```bash
nc -vz example.com 443
```

### Download a file

```bash
wget https://example.com/file.txt
```

`curl`, `wget`, and `nc` may need to be installed on a minimal system.

### Public IP address

```bash
curl -s https://checkip.amazonaws.com
```

This requests the public IP observed by the external service. On EC2, the instance's public IPv4 address and its private address are different.

---

## 15. Package Management in Ubuntu

Ubuntu uses APT for package management.

### Refresh package lists

```bash
sudo apt update
```

### Upgrade installed packages

```bash
sudo apt upgrade
```

Review package changes before confirming the upgrade.

### Install a package

```bash
sudo apt install tree
```

### Remove a package

```bash
sudo apt remove tree
```

### Search for a package

```bash
apt search nginx
```

### Display package information

```bash
apt show nginx
```

### List installed packages

```bash
apt list --installed
```

### Check whether a package is installed

```bash
dpkg -l nginx
```

**Remember:** `apt update` refreshes package lists; `apt upgrade` installs available upgrades.

---

## 16. Archives and Compression

### Create a tar archive

```bash
tar -cvf backup.tar project/
```

### Extract a tar archive

```bash
tar -xvf backup.tar
```

### Create a compressed gzip archive

```bash
tar -czvf backup.tar.gz project/
```

### Extract a gzip-compressed tar archive

```bash
tar -xzvf backup.tar.gz
```

### List archive contents without extracting

```bash
tar -tvf backup.tar
```

### ZIP a directory

```bash
zip -r backup.zip project/
```

### Unzip an archive

```bash
unzip backup.zip
```

`zip` and `unzip` may need to be installed separately.

---

## 17. Users and Groups

### Show current user

```bash
whoami
```

### Show user and group IDs

```bash
id
```

### List local user account entries

```bash
cat /etc/passwd
```

### List local group entries

```bash
cat /etc/group
```

### Create a user

```bash
sudo adduser devuser
```

### Create a group

```bash
sudo groupadd developers
```

### Add a user to a supplementary group

```bash
sudo usermod -aG developers devuser
```

The `-a` option preserves existing supplementary group memberships when used with `-G`.

### Switch to another user

```bash
su - devuser
```

You need the target user's credentials or appropriate administrative access.

### Run a command with administrative privileges

```bash
sudo command
```

Example:

```bash
sudo systemctl status cron
```

---

## 18. Command History and Terminal Shortcuts

| Command or shortcut | Purpose |
|---|---|
| `history` | Show previous commands |
| `history \| tail` | Show recent history entries |
| `!number` | Execute a command by its history number |
| `!!` | Repeat the previous command |
| `Ctrl + R` | Search command history interactively |
| `Up Arrow` | Recall the previous command |
| `Down Arrow` | Move forward through command history |
| `Ctrl + C` | Interrupt the current foreground command |
| `Ctrl + D` | Send end-of-input; may exit a shell |
| `Ctrl + L` | Clear the visible terminal screen |
| `clear` | Clear the visible terminal screen |
| `Tab` | Complete filenames and commands |
| `Ctrl + A` | Move to the beginning of the command line in common shells |
| `Ctrl + E` | Move to the end of the command line in common shells |

Be careful with `!!` and `!number`: they execute a command, not merely display it.

---

## 19. Important Linux Operators

### Pipe `|`

Send the output of one command to another command.

```bash
ls -l | less
```

### Redirect output `>`

Create or overwrite an output file.

```bash
ls -l > files.txt
```

### Append output `>>`

Append output to an existing file.

```bash
date >> dates.txt
```

### Run the next command only if the first succeeds `&&`

```bash
mkdir project && cd project
```

### Run the next command if the first fails `||`

```bash
mkdir project || echo "Directory creation failed"
```

### Run commands sequentially with `;`

```bash
pwd; date; whoami
```

The second and third commands run even if an earlier command fails.

### Wildcards

```bash
ls *.txt
```

Matches names ending in `.txt` in the current directory.

### Environment variable

```bash
echo "$HOME"
```

Displays the home directory of the current user.

---

## 20. Essential AWS EC2 Troubleshooting Commands

These examples are useful when connected to an Ubuntu EC2 instance using SSH.

### Check the current user and directory

```bash
whoami
pwd
hostname
```

### Check operating system and kernel

```bash
cat /etc/os-release
uname -r
```

### Check disk and memory

```bash
df -h
free -h
```

### Check uptime and load average

```bash
uptime
```

### Check a service

```bash
sudo systemctl status cron --no-pager
```

### Check recent system logs

```bash
sudo journalctl -n 50 --no-pager
```

### Follow the Ubuntu system log, if present

```bash
sudo tail -f /var/log/syslog
```

### Check open listening ports

```bash
sudo ss -tulpn
```

### Check Cron configuration for your user

```bash
crontab -l
```

### Check recent Cron log messages

```bash
sudo grep CRON /var/log/syslog | tail -n 20
```

### Monitor your project log

```bash
tail -f cron.log
```

Run this last command from the directory containing `cron.log`.

---

## 21. Safe Hands-on Practice

Run the following commands on your Ubuntu EC2 instance. These exercises create a separate practice directory and avoid deleting existing files.

### Step 1: Create a practice directory

```bash
mkdir -p ~/linux-practice
cd ~/linux-practice
pwd
```

### Step 2: Create files and folders

```bash
mkdir -p project/{src,docs,logs}
touch project/docs/notes.txt
touch project/logs/app.log
```

### Step 3: Add sample content

```bash
echo "Linux practice" > project/docs/notes.txt
echo "INFO Application started" > project/logs/app.log
echo "ERROR Sample error" >> project/logs/app.log
```

### Step 4: Inspect the directory structure

```bash
ls -lah
ls -R project
```

### Step 5: Practise file viewing

```bash
cat project/docs/notes.txt
head -n 2 project/logs/app.log
tail -n 2 project/logs/app.log
less project/logs/app.log
```

Press `q` to exit `less`.

### Step 6: Search for errors

```bash
grep -in "error" project/logs/app.log
```

### Step 7: Check file permissions

```bash
ls -l project/docs/notes.txt
chmod 600 project/docs/notes.txt
ls -l project/docs/notes.txt
```

### Step 8: Check system resources

```bash
free -h
df -h
uptime
```

### Step 9: Copy and rename a file

```bash
cp project/docs/notes.txt notes-backup.txt
mv notes-backup.txt notes-copy.txt
ls -l
```

### Step 10: Verify your practice files

```bash
find ~/linux-practice -type f
```

Keep this practice directory for revision and add more exercises as you learn.

---

## 22. Interview Questions

1. What is the difference between an absolute path and a relative path?
2. What is the difference between `.` and `..`?
3. What is the difference between `mkdir` and `mkdir -p`?
4. What is the difference between `touch` and `cat`?
5. What is the difference between `>` and `>>`?
6. What is the difference between `head` and `tail`?
7. Why is `less` useful for viewing large log files?
8. What is the difference between `grep` and `find`?
9. What is the difference between `cp` and `mv`?
10. What is the difference between `rm` and `rmdir`?
11. What do `r`, `w`, and `x` mean in Linux permissions?
12. What is the difference between `chmod` and `chown`?
13. What is the difference between `df -h` and `du -sh`?
14. What is the purpose of `free -h`?
15. What does `uptime` show?
16. What is the difference between `apt update` and `apt upgrade`?
17. How can you inspect a running service using `systemctl`?
18. How can you monitor a log file continuously?
19. What does a pipe (`|`) do?
20. Why should `rm -rf` be used carefully?

---

## 23. Quick Revision Cheat Sheet

| Requirement | Command |
|---|---|
| Current directory | `pwd` |
| Current user | `whoami` |
| Create directory | `mkdir folder` |
| Create nested directories | `mkdir -p a/b/c` |
| Create file | `touch file.txt` |
| List files | `ls -la` |
| Change directory | `cd /path` |
| Go up one level | `cd ..` |
| Read a small file | `cat file.txt` |
| First 10 lines | `head file.txt` |
| Last 10 lines | `tail file.txt` |
| Follow log live | `tail -f app.log` |
| Browse large file | `less file.txt` |
| Search text | `grep "word" file.txt` |
| Find files | `find . -name "*.log"` |
| Copy file | `cp source destination` |
| Move or rename file | `mv old new` |
| Delete file | `rm file.txt` |
| View permissions | `ls -l` |
| Change permissions | `chmod 644 file.txt` |
| Disk space | `df -h` |
| Directory size | `du -sh folder` |
| RAM | `free -h` |
| CPU information | `lscpu` |
| Load average | `uptime` |
| Processes | `ps aux` |
| Service status | `systemctl status cron` |
| IP information | `ip addr` |
| Routing table | `ip route` |
| Date and time | `date` |
| OS information | `cat /etc/os-release` |
| Command history | `history` |
| Clear terminal | `Ctrl + L` |

---

## My Linux Learning Checklist

- [ ] Learn directory navigation.
- [ ] Practise creating files and directories.
- [ ] Understand `ls` options.
- [ ] Learn `cat`, `head`, `tail`, `more`, and `less`.
- [ ] Practise copying, moving, and deleting files safely.
- [ ] Understand output redirection and pipes.
- [ ] Learn `grep` and `find`.
- [ ] Understand Linux permissions and ownership.
- [ ] Learn system monitoring commands.
- [ ] Practise service and log troubleshooting.
- [ ] Learn basic networking commands.
- [ ] Practise package installation with APT.
- [ ] Complete the hands-on exercises.
- [ ] Revise the interview questions.

**Learning path:** Linux Basics → Filesystem → File Permissions → Text Processing → Processes and Services → Networking → Bash Scripting → AWS → DevOps Automation.

*Note: These commands are primarily written for Ubuntu. Some packages, utilities, log paths, and command options may differ on RHEL and other Linux distributions.*
