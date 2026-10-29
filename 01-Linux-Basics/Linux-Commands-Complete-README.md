# Linux Commands — Complete Beginner-to-DevOps Reference

> **Platform:** Ubuntu Linux (including AWS EC2)  
> **Goal:** Learn, practise, revise, and troubleshoot Linux confidently.  
> **How to use:** Read one section, run its safe examples, and record what each command does.

## Table of Contents
1. [Linux and terminal basics](#1-linux-and-terminal-basics)
2. [Navigation and paths](#2-navigation-and-paths)
3. [Directories and files](#3-directories-and-files)
4. [The `ls` command in detail](#4-the-ls-command-in-detail)
5. [Reading files: `cat`, `head`, `tail`, `more`, `less`](#5-reading-files-cat-head-tail-more-less)
6. [Copy, move, rename, and delete](#6-copy-move-rename-and-delete)
7. [Search files and text](#7-search-files-and-text)
8. [Redirection, pipes, and text processing](#8-redirection-pipes-and-text-processing)
9. [Permissions, ownership, and links](#9-permissions-ownership-and-links)
10. [System and hardware information](#10-system-and-hardware-information)
11. [Disk, memory, and CPU](#11-disk-memory-and-cpu)
12. [Processes and jobs](#12-processes-and-jobs)
13. [Services, systemd, and logs](#13-services-systemd-and-logs)
14. [Networking and SSH](#14-networking-and-ssh)
15. [Packages and software](#15-packages-and-software)
16. [Archives and compression](#16-archives-and-compression)
17. [Users and groups](#17-users-and-groups)
18. [Environment variables, history, and shortcuts](#18-environment-variables-history-and-shortcuts)
19. [Scheduling with cron](#19-scheduling-with-cron)
20. [Shell scripting basics](#20-shell-scripting-basics)
21. [AWS EC2 and DevOps troubleshooting](#21-aws-ec2-and-devops-troubleshooting)
22. [Ubuntu vs RHEL command differences](#22-ubuntu-vs-rhel-command-differences)
23. [Safe hands-on lab](#23-safe-hands-on-lab)
24. [Interview questions](#24-interview-questions)
25. [Quick revision cheat sheet](#25-quick-revision-cheat-sheet)

---

## 1. Linux and terminal basics

Linux is an operating system kernel. Ubuntu and Red Hat Enterprise Linux (RHEL) are Linux distributions. A **shell** interprets commands; **Bash** is a common shell. A **terminal** is the application through which you interact with the shell.

```bash
whoami           # Current username
hostname         # Machine hostname
pwd              # Print working directory
clear            # Clear terminal screen
date             # Current date and time
cal              # Calendar (may require installing ncal)
uname -a         # Kernel/system information
cat /etc/os-release  # Distribution and version
man ls           # Manual for ls (if man is installed)
ls --help        # Command-specific help
type ls          # How the shell resolves ls
command -v ls    # Path to executable, if applicable
```

**Prompt example:** `ubuntu@ip-172-31-2-84:~/project/santhosh$` means username `ubuntu`, hostname `ip-172-31-2-84`, current directory `~/project/santhosh`, and `$` indicates a regular-user shell. A root shell often shows `#`.

**Memory trick:** `pwd` = **p**rint **w**orking **d**irectory.

## 2. Navigation and paths

```bash
pwd                 # Show current directory
cd /var/log         # Go to an absolute path
cd ~                # Go to your home directory
cd ..               # Go to parent directory
cd ../..            # Go up two levels
cd -                # Return to previous directory
cd .                # Stay in current directory
ls /etc             # List a specific directory
```

- **Absolute path:** starts with `/`, such as `/home/ubuntu/project`.
- **Relative path:** starts from your current directory, such as `project/logs`.
- `~` means your home directory; `.` means current directory; `..` means parent directory.

Important filesystem locations:

| Path | Purpose |
|---|---|
| `/` | Filesystem root |
| `/home` | Regular users' home directories |
| `/root` | Root user's home directory |
| `/etc` | System configuration |
| `/var/log` | System and application logs |
| `/var` | Variable application/system data |
| `/tmp` | Temporary files |
| `/usr/bin` | Many user commands |
| `/bin` | Essential commands (often merged into `/usr/bin`) |
| `/opt` | Optional third-party software |
| `/dev` | Device files |
| `/proc` | Kernel/process information (virtual filesystem) |
| `/sys` | Device/kernel interface (virtual filesystem) |
| `/mnt`, `/media` | Mount points |

## 3. Directories and files

```bash
mkdir demo                   # Create directory
mkdir -p demo/logs/archive   # Create nested directories
rmdir empty-folder          # Remove an empty directory
touch notes.txt             # Create empty file/update timestamp
file notes.txt              # Identify file type
stat notes.txt              # Show file metadata
wc -l notes.txt             # Count lines
wc -w notes.txt             # Count words
wc -c notes.txt             # Count bytes
```

Use a text editor:

```bash
nano notes.txt              # Beginner-friendly editor (if installed)
vi notes.txt                # Vi/Vim-style editor
```

**Nano:** `Ctrl+O` to save, Enter to confirm, `Ctrl+X` to exit. **Vi:** press `i` to insert, `Esc` then `:wq` to save and quit; `:q!` quits without saving.

## 4. The `ls` command in detail

**Purpose:** List directory contents. By default, names beginning with `.` are hidden.

**Syntax:** `ls [OPTION]... [FILE]...`

### Everyday `ls` commands

| Command | Meaning |
|---|---|
| `ls` | List visible entries |
| `ls -l` | Detailed/long listing |
| `ls -a` | Include hidden entries, including `.` and `..` |
| `ls -A` | Include hidden entries except `.` and `..` |
| `ls -la` | Detailed listing, including hidden entries |
| `ls -lh` | Long listing with human-readable sizes |
| `ls -lah` | Long listing, hidden entries, readable sizes |
| `ls -lt` | Newest modified entries first |
| `ls -ltr` | Oldest modified entries first |
| `ls -lS` | Largest file size first |
| `ls -R` | Recursively list subdirectories |
| `ls -d */` | List subdirectory names (shell glob) |
| `ls -ld folder/` | Show directory's own metadata |
| `ls -i` | Show inode numbers |
| `ls -F` | Append type markers (`/` directory, `@` symlink, `*` executable) |
| `ls -1` | One entry per line |
| `ls -r` | Reverse sort order |
| `ls -X` | Sort by extension |
| `ls -v` | Natural/version sorting |
| `ls --group-directories-first` | Put directories first |
| `ls --color=auto` | Colorize when supported |

### Additional options from `ls --help`

| Option | Meaning |
|---|---|
| `-C`, `-x`, `-m` | Columns, rows, or comma-separated output |
| `-o`, `-g`, `-G` | Long listing without group, without owner, or without group field |
| `-n` | Numeric UID/GID instead of names |
| `-h`, `--si` | Human-readable size using 1024 or 1000 bases |
| `-s`, `-k` | Show allocated size; use KiB blocks |
| `-t`, `-S`, `-U`, `-X`, `-v` | Sort by time, size, unsorted, extension, version |
| `-u`, `-c`, `--time=FIELD` | Use access time, metadata change time, or selected timestamp |
| `--time-style=long-iso`, `--full-time` | Control timestamp display |
| `-I PATTERN`, `--hide=PATTERN`, `-B` | Ignore matching names, hide matches, ignore backups |
| `-d`, `-R`, `-L`, `-H` | Directory itself, recursive, dereference symlinks, follow command-line symlinks |
| `-p`, `-F`, `--file-type` | Append directory/type indicators |
| `-b`, `-N`, `-Q`, `-q` | Escaping/quoting and control-character display |
| `--hyperlink=auto`, `--zero` | Terminal hyperlinks, NUL-separated entries |
| `-Z`, `--author`, `--dired` | Security context, author, Emacs dired format |
| `--help`, `--version` | Help and version |

Some options vary by `ls` implementation/version; always use `ls --help` on your own machine. In particular, `--format`, `--sort`, `--indicator-style`, `--tabsize`, and `--width` provide extra formatting control.

### Understand `ls -l` output

```text
-rw-r--r-- 1 ubuntu ubuntu 2048 Oct 28 10:30 report.txt
```

- `-` = regular file (`d` means directory, `l` means symbolic link).
- `rw-r--r--` = owner, group, and others permissions.
- `1` = hard-link count.
- First `ubuntu` = owner; second `ubuntu` = group.
- `2048` = size in bytes; date/time = modification time; `report.txt` = name.

**Practice:** `ls -lah /var/log` and `ls -lt /var/log | head`.

## 5. Reading files: `cat`, `head`, `tail`, `more`, `less`

These commands are especially important for viewing application logs on Ubuntu and AWS EC2.

| Command | What it does | Best use |
|---|---|---|
| `cat file.txt` | Print entire file | Small files |
| `cat -n file.txt` | Print with line numbers | Review configuration |
| `head file.txt` | First 10 lines by default | Inspect file header |
| `head -n 20 file.txt` | First 20 lines | View beginning |
| `tail file.txt` | Last 10 lines by default | Recent log entries |
| `tail -n 50 file.txt` | Last 50 lines | Recent errors |
| `tail -f app.log` | Follow newly appended lines | Live log monitoring |
| `tail -F app.log` | Follow by filename; retry after rotation | Rotating logs |
| `more file.txt` | Page forward through text | Basic paging |
| `less file.txt` | Scroll/search through text | Large logs |

```bash
cat /etc/os-release
head -n 5 /etc/passwd
tail -n 20 /var/log/dpkg.log
sudo journalctl -u ssh -n 30 --no-pager
sudo journalctl -u ssh -f
```

**Inside `less`:** `Space` next page; `b` previous page; `/error` search; `n` next match; `g` start; `G` end; `q` quit. **Inside `more`:** `Space` next page; `q` quit. Stop `tail -f` with `Ctrl+C`.

**Memory trick:** `head` = beginning; `tail` = end; `less` = inspect and search; `tail -f` = watch live.

## 6. Copy, move, rename, and delete

```bash
cp source.txt backup.txt          # Copy a file
cp -i source.txt backup.txt       # Ask before overwrite
cp -r source-dir/ copied-dir/     # Copy directory recursively
mv old.txt new.txt                # Rename file
mv report.txt archive/           # Move file into folder
mv -i old.txt new.txt            # Ask before overwrite
rm -i old.txt                    # Ask before removing file
rmdir empty-dir                  # Remove empty directory
```

**Danger:** `rm` deletes files without moving them to a recycle bin. `rm -r` deletes directory trees; `rm -rf` forces recursive deletion. Never run deletion commands on unfamiliar paths, especially with `sudo`, globs, or `/`.

## 7. Search files and text

```bash
find . -name '*.log'             # Find .log files under current directory
find /var/log -type f -name '*.log'  # Search regular log files
find . -type d -name 'config'     # Find directories named config
find . -type f -mtime -1         # Files modified in last 24 hours
grep 'ERROR' app.log             # Find matching lines
grep -i 'error' app.log          # Ignore case
grep -n 'error' app.log          # Show line numbers
grep -R 'timeout' ./config       # Recursive search
grep -E 'ERROR|WARN' app.log     # Extended regex
command -v python3              # Find executable via shell PATH
```

**Tip:** Quote patterns such as `'*.log'` to prevent your shell from expanding them before `find` runs.

## 8. Redirection, pipes, and text processing

```bash
echo 'Hello Linux'               # Print text
echo 'Hello' > demo.txt          # Write/overwrite file
echo 'Again' >> demo.txt         # Append to file
ls /missing 2> errors.txt        # Redirect standard error
command > output.txt 2>&1       # Redirect stdout and stderr
cat demo.txt | wc -l            # Pipe output to next command
grep 'ERROR' app.log | tail -n 10
sort names.txt                   # Sort lines
sort names.txt | uniq            # Collapse adjacent duplicates
sort names.txt | uniq -c         # Count repeated lines
cut -d: -f1 /etc/passwd         # Extract first colon-separated field
awk '{print $1}' data.txt        # Print first whitespace-separated field
sed -n '1,10p' data.txt         # Print first ten lines
tee copy.txt < demo.txt          # Display input and save a copy
```

`>` overwrites; `>>` appends; `|` passes output to another command; `2>` redirects errors. Prefer `grep pattern file` over `cat file | grep pattern` when no pipeline is needed.

## 9. Permissions, ownership, and links

```bash
ls -l script.sh
chmod +x script.sh               # Add executable permission
chmod 644 notes.txt              # Owner rw; group/others r
chmod 755 script.sh              # Owner rwx; group/others rx
chmod u+x script.sh              # Add execute for owner
sudo chown ubuntu:ubuntu notes.txt  # Change owner and group
chgrp developers notes.txt       # Change group (if permitted)
ln -s notes.txt notes-link.txt   # Symbolic link
readlink notes-link.txt          # Display link target
```

Permission values: `r=4`, `w=2`, `x=1`. Thus `7=rwx`, `6=rw-`, `5=r-x`, `4=r--`. Use least privilege; avoid `chmod 777` as a routine fix. Directory permissions have special traversal semantics: `x` allows entering/traversing a directory.

## 10. System and hardware information

```bash
hostnamectl              # Hostname and OS details
uname -r                 # Kernel release
uname -a                 # Full kernel details
cat /etc/os-release      # Distribution version
uptime                   # Uptime and load averages
date                     # System date/time
timedatectl              # Clock/timezone information
lscpu                    # CPU information
lsblk                    # Block devices and mount points
```

## 11. Disk, memory, and CPU

```bash
df -h                    # Filesystem free/used space
df -i                    # Inode usage
du -sh .                 # Total size of current directory
du -sh /var/log          # Size of log directory
free -h                  # RAM and swap usage
vmstat 1 5               # Memory/CPU snapshots (if installed)
top                      # Live process/system monitor
ps aux --sort=-%mem | head  # Top memory-consuming processes
ps aux --sort=-%cpu | head  # Top CPU-consuming processes
```

**Difference:** `df` reports filesystem capacity; `du` reports file/directory space usage. Linux `free` memory includes cache; `available` is generally more useful than the `free` column for assessing memory headroom.

## 12. Processes and jobs

```bash
ps aux                   # All visible processes
ps -ef                   # Alternative full process listing
pgrep -a python3         # Search process names and show command lines
kill -TERM 1234          # Request graceful termination
kill -KILL 1234          # Force termination only as last resort
jobs                     # Shell background jobs
sleep 60 &               # Run job in background
fg                       # Bring shell job to foreground
bg                       # Resume stopped shell job in background
nohup python3 app.py > app.log 2>&1 &  # Keep running after logout
```

Use `Ctrl+C` to interrupt a foreground command, `Ctrl+Z` to suspend it. Prefer `systemd` services for long-running production workloads rather than relying only on `nohup`.

## 13. Services, systemd, and logs

```bash
systemctl status ssh             # Status of SSH service on Ubuntu
sudo systemctl start ssh         # Start service
sudo systemctl stop ssh          # Stop service (careful on remote EC2!)
sudo systemctl restart nginx     # Restart Nginx if installed
sudo systemctl enable nginx      # Enable at boot
sudo systemctl disable nginx     # Disable at boot
systemctl list-units --type=service --state=running
journalctl -u nginx -n 50 --no-pager  # Last 50 service log entries
sudo journalctl -u nginx -f      # Follow service logs
journalctl -b -p err --no-pager  # Errors since boot
```

**Important:** Do not stop or restart SSH while connected to a remote server unless you have an alternative recovery path. Service names can differ by distro (e.g., `ssh` on Ubuntu vs `sshd` on RHEL).

## 14. Networking and SSH

```bash
ip addr                         # Network addresses
ip route                        # Routing table
hostname -I                     # Host IP addresses
ping -c 4 8.8.8.8              # Connectivity test
ping -c 4 example.com          # DNS + connectivity test
curl -I https://example.com     # HTTP response headers
curl -s https://checkip.amazonaws.com  # Public IP from external service
ss -tuln                        # Listening TCP/UDP ports
ss -tulpn                       # Listening ports + process info (permissions vary)
getent hosts example.com        # Resolve hostname
ssh ubuntu@SERVER_PUBLIC_IP     # SSH login (requires auth/access)
ssh -i key.pem ubuntu@SERVER_PUBLIC_IP  # SSH using a private key
scp -i key.pem report.txt ubuntu@SERVER_PUBLIC_IP:~/  # Copy to server
```

For EC2, check **security groups**, **network ACLs**, **route tables**, **instance firewall**, **service binding address**, and **whether the process is listening**. Do not share private SSH keys. Some utilities (e.g., `ping`) may need installation; ICMP may be blocked even when HTTP works.

## 15. Packages and software

### Ubuntu / Debian (APT)

```bash
sudo apt update                 # Refresh package indexes
apt list --upgradable          # Show available upgrades
sudo apt upgrade                # Upgrade installed packages
sudo apt install nginx          # Install a package
sudo apt remove nginx           # Remove package, keep some config
sudo apt purge nginx            # Remove package and packaged config
sudo apt autoremove             # Remove unused dependencies
apt show nginx                  # Package details
dpkg -l | grep nginx            # Query installed packages
```

Only install or remove software you understand on shared/production systems. `apt update` updates package metadata; it does not upgrade installed packages.

### RHEL / Fedora

```bash
sudo dnf check-update           # Check available updates
sudo dnf install nginx          # Install
sudo dnf upgrade                # Upgrade packages
sudo dnf remove nginx           # Remove
rpm -qa | grep nginx            # Query installed RPMs
```

## 16. Archives and compression

```bash
tar -czf backup.tar.gz project/    # Create compressed tar archive
tar -tzf backup.tar.gz             # List archive contents
tar -xzf backup.tar.gz             # Extract into current directory
tar -xzf backup.tar.gz -C /tmp     # Extract into /tmp
gzip file.txt                      # Compress file (replaces original)
gunzip file.txt.gz                 # Decompress
zip -r backup.zip project/         # Zip folder (if installed)
unzip -l backup.zip                # List zip contents (if installed)
```

**Memory trick:** `tar`: `c=create`, `x=extract`, `t=list`, `z=gzip`, `f=file`.

## 17. Users and groups

```bash
whoami                         # Current user
id                             # UID, GID, groups
who                            # Logged-in users
groups                         # Your groups
getent passwd ubuntu           # Account entry
sudo adduser trainee           # Create account (Ubuntu)
sudo usermod -aG sudo trainee   # Add to sudo group (Ubuntu)
sudo passwd trainee            # Set/change password
sudo -l                        # View allowed sudo actions
```

Account changes require authorization. On many RHEL systems, administrative users belong to the `wheel` group instead of `sudo`.

## 18. Environment variables, history, and shortcuts

```bash
history                        # Command history
history | tail -n 20           # Recent commands
printenv PATH                  # Display PATH
echo "$HOME"                   # Home directory
export APP_ENV=dev             # Set variable for current shell/session
alias ll='ls -lah'             # Temporary alias
unalias ll                     # Remove alias
```

| Shortcut | Purpose |
|---|---|
| `Tab` | Autocomplete commands and paths |
| `Ctrl+C` | Interrupt foreground command |
| `Ctrl+L` | Clear visible terminal |
| `Ctrl+A` / `Ctrl+E` | Start/end of command line |
| `Ctrl+R` | Search command history |
| `Ctrl+U` | Delete line before cursor |
| `Ctrl+K` | Delete line after cursor |
| `Ctrl+D` | End input/logout from shell |

**Warning:** Shell history may retain commands containing secrets. Avoid putting credentials directly on the command line.

## 19. Scheduling with cron

```bash
crontab -l                    # View your crontab
crontab -e                    # Edit your crontab
systemctl status cron        # Cron daemon status on Ubuntu
```

Cron fields: `minute hour day-of-month month day-of-week command`.

```cron
# Run every 5 minutes (example; adjust the absolute script path)
*/5 * * * * /usr/bin/python3 /home/ubuntu/monitor.py >> /home/ubuntu/monitor.log 2>&1
```

Use **absolute paths** because cron runs with a limited environment. Ensure your script and log destination exist and permissions are correct. Cron runs in the server's configured timezone.

## 20. Shell scripting basics

Create `hello.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

name="Linux learner"
printf 'Hello, %s!\n' "$name"
printf 'Today is %s\n' "$(date +%F)"
```

Run it:

```bash
bash hello.sh
chmod +x hello.sh
./hello.sh
```

Common scripting tools: `if`, `for`, `while`, `case`, functions, `read`, positional parameters (`$1`, `$2`), exit status (`$?`), and `test`/`[[ ... ]]`. `set -euo pipefail` can help catch some errors, but has nuances; always test scripts carefully.

## 21. AWS EC2 and DevOps troubleshooting

### Quick health checks

```bash
uptime                      # Load averages
free -h                     # RAM
df -h                       # Disk capacity
df -i                       # Inodes
top                         # Live resource usage
systemctl --failed          # Failed systemd units
sudo journalctl -p err -b --no-pager | tail -n 30
ss -tuln                    # Listening ports
```

### Example: Website does not open

1. Check whether the EC2 instance is running and its public IP/DNS is correct.
2. Check EC2 security group inbound rules for the required port (typically 80/443); limit SSH access to trusted IPs.
3. Check whether the web server is active: `systemctl status nginx`.
4. Check listening ports: `sudo ss -tulpn | grep -E ':80|:443'`.
5. Test locally: `curl -I http://127.0.0.1`.
6. Inspect logs: `sudo journalctl -u nginx -n 50 --no-pager` and, if present, `/var/log/nginx/error.log`.
7. Check network ACLs, route tables, public IP association, and firewall rules if necessary.

### Example: Server is slow

1. `uptime` — check load averages.
2. `top` — identify CPU/memory-intensive processes.
3. `free -h` — check available RAM and swap.
4. `df -h` and `df -i` — check disk space and inode exhaustion.
5. `journalctl -p err -b` — review errors.

### Example: Monitor a custom application log

```bash
tail -n 30 ~/project/app.log
tail -f ~/project/app.log
grep -in 'error\|failed' ~/project/app.log | tail -n 20
```

If the file does not exist, substitute the actual path. Avoid exposing real logs, secrets, access tokens, or customer data in public dashboards or repositories.

## 22. Ubuntu vs RHEL command differences

| Task | Ubuntu | RHEL |
|---|---|---|
| Package manager | `apt` | `dnf` |
| Low-level package query | `dpkg -l` | `rpm -qa` |
| Install Nginx | `sudo apt install nginx` | `sudo dnf install nginx` |
| SSH service name | Often `ssh` | Often `sshd` |
| Admin group | Usually `sudo` | Usually `wheel` |
| Default firewall tooling | Often `ufw` (if enabled) | Often `firewalld` |
| Mandatory access control | AppArmor common | SELinux common |

Most general commands (`ls`, `cd`, `grep`, `find`, `df`, `ps`, `systemctl`, `journalctl`) work on both, though available versions/options and configurations can differ.

## 23. Safe hands-on lab

Run the following in your own home directory, not in a production application folder:

```bash
mkdir -p ~/linux-practice/logs
cd ~/linux-practice
printf 'INFO startup\nWARN slow\nERROR timeout\nINFO retry\n' > logs/app.log
pwd
ls -lah
cat -n logs/app.log
head -n 2 logs/app.log
tail -n 2 logs/app.log
grep -n 'ERROR' logs/app.log
cp logs/app.log logs/app-backup.log
ls -lh logs/
wc -l logs/app.log
find . -type f -name '*.log'
du -sh .
```

**Challenge:** Use `less logs/app.log` to search for `ERROR`, use `tail -f logs/app.log` in one terminal, and append a new line from a second terminal with `echo 'INFO healthy' >> ~/linux-practice/logs/app.log`. Stop following with `Ctrl+C`.

## 24. Interview questions

1. **What is the difference between `pwd` and `cd`?** `pwd` prints your location; `cd` changes it.
2. **What is the difference between `ls -a` and `ls -l`?** `-a` includes hidden entries; `-l` shows detailed metadata.
3. **What does `ls -lah` mean?** Long listing, all entries, human-readable sizes.
4. **When would you use `head` vs `tail`?** Beginning of a file vs most recent/end of a file.
5. **What does `tail -f` do?** Watches newly appended log lines.
6. **Why use `less` instead of `cat` for large logs?** Paging, navigation, and searching.
7. **What is the difference between `>` and `>>`?** Overwrite vs append.
8. **What is a pipe (`|`)?** Sends one command's standard output to another command's standard input.
9. **What does `chmod 755` mean?** Owner rwx, group rx, others rx.
10. **Difference between `df -h` and `du -sh`?** Filesystem usage vs directory/file usage.
11. **How do you see running processes?** `ps aux` or `top`.
12. **How do you troubleshoot a failing service?** `systemctl status`, `journalctl -u`, process/port checks, and application logs.
13. **What does `sudo apt update` do?** Refreshes available package indexes.
14. **How do you find a file by name?** `find /path -name 'pattern'`.
15. **How do you check listening ports?** `ss -tuln`.
16. **How do you run a script every five minutes?** Cron entry `*/5 * * * * ...`.
17. **What is the difference between absolute and relative paths?** Absolute starts at `/`; relative starts at current location.
18. **Why might a website fail even when Nginx is running?** Security groups, firewalls, routes, binding, DNS, or application errors.

## 25. Quick revision cheat sheet

| Goal | Command |
|---|---|
| Current location | `pwd` |
| Change directory | `cd /path` |
| List files | `ls -lah` |
| New folder | `mkdir -p folder/subfolder` |
| New file | `touch file.txt` |
| Read file | `cat file.txt` |
| First lines | `head -n 20 file.txt` |
| Last lines | `tail -n 20 file.txt` |
| Live logs | `tail -f app.log` |
| Page/search file | `less file.txt` |
| Copy | `cp source destination` |
| Rename/move | `mv old new` |
| Search text | `grep -in 'error' app.log` |
| Find file | `find . -name '*.log'` |
| Permissions | `chmod 755 script.sh` |
| Disk space | `df -h` |
| Folder size | `du -sh folder` |
| RAM | `free -h` |
| CPU/processes | `top` |
| Service status | `systemctl status nginx` |
| Service logs | `journalctl -u nginx -n 50` |
| Ports | `ss -tuln` |
| IP addresses | `ip addr` |
| OS version | `cat /etc/os-release` |
| Package index refresh | `sudo apt update` |
| Command help | `man command` or `command --help` |

---

### Recommended learning order

**Week 1:** Sections 1–6 (navigation, `ls`, file viewing, file operations).  
**Week 2:** Sections 7–9 (search, pipes, permissions).  
**Week 3:** Sections 10–17 (system administration and networking).  
**Week 4:** Sections 18–23 (scripting, cron, EC2 troubleshooting, hands-on labs).

**GitHub suggestion:** Save this as `Linux-DevOps-Notes/Linux-Commands/README.md`. Practise commands in a safe folder and add screenshots or your own observations after each lab.
