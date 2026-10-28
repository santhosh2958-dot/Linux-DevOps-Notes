
# 11 - Advanced Linux Commands: Complete DevOps Guide

Welcome to the Advanced Linux Commands guide!

This guide is designed for beginners who already know basic Linux commands and want to develop practical Linux administration and DevOps troubleshooting skills.

You will learn what each command does, why it is useful, how to use it, and where you might use it on a real Linux server.

**Learning objective:** Become comfortable investigating processes, disk usage, networking, system logs, permissions, services, and performance problems on Linux servers.

> Practice these commands on Ubuntu or a test EC2 instance. Some commands require `sudo`, and some are available only on particular Linux distributions.

---

# Table of Contents

1. What Are Advanced Linux Commands?
2. Understanding Command Syntax
3. Getting System Information
4. Process Management
5. CPU, Memory and Performance
6. Disk and Filesystem Management
7. File Searching and Text Processing
8. Logs and Troubleshooting
9. Networking Commands
10. User, Permission and Security Commands
11. Services and System Administration
12. Archives and Compression
13. SSH and Remote Administration
14. Useful Command Combinations
15. Advanced Command Safety
16. Mini Project: Linux Server Diagnostic Report
17. Hands-On Practice Labs
18. Troubleshooting Scenarios
19. Linux Interview Questions and Answers
20. Quick Revision Cheat Sheet
21. Learning Checklist
22. Official Learning Resources

---

# 1. What Are Advanced Linux Commands?

Advanced Linux commands help you investigate and manage a Linux system beyond basic file and directory operations.

Basic commands include:

```bash
pwd
ls
cd
mkdir
touch
```

Advanced administration and troubleshooting commands include:

```bash
ps
top
free
df
du
find
grep
awk
sed
ss
ip
journalctl
systemctl
lsof
```

These commands are particularly useful when managing Linux servers, applications, virtual machines, cloud instances, and production environments.

## Why do DevOps engineers need these commands?

Imagine that a website running on an EC2 instance becomes slow.

You may need to investigate:

- Is the CPU overloaded?
- Is memory running out?
- Is the disk full?
- Is the application process running?
- Is the application listening on the expected port?
- Are there errors in the logs?
- Is a required service stopped?
- Is the application unable to connect to another server?

Linux commands help you gather evidence before deciding what action to take.

## A simple troubleshooting workflow

```text
Application is slow
        |
        v
Check CPU and memory
        |
        v
Check disk space
        |
        v
Check application process
        |
        v
Check service status and logs
        |
        v
Check network connectivity
        |
        v
Identify the cause and take an appropriate action
```

**Important:** A command provides information or performs a specific operation. Correct troubleshooting requires interpreting its output and understanding the application.

---

# 2. Understanding Command Syntax

Most Linux commands follow a pattern like this:

```text
command [options] [arguments]
```

Example:

```bash
ls -lah /var/log
```

| Part | Meaning |
|---|---|
| `ls` | Command |
| `-l` | Long listing format |
| `-a` | Include hidden entries |
| `-h` | Human-readable sizes |
| `/var/log` | Directory argument |

You can often combine short options, as shown above.

## How to learn an unfamiliar command

Use:

```bash
command --help
```

Example:

```bash
df --help
```

For detailed documentation:

```bash
man df
```

Inside a manual page:

- Press `/` to search.
- Press `n` to find the next match.
- Press `q` to quit.

You can also inspect the command's location:

```bash
command -v grep
```

And identify its type:

```bash
type cd
type ls
```

Some commands are built into the shell, while others are separate executable programs.

---

# 3. Getting System Information

These commands help you understand the Linux machine you are managing.

## 3.1 `uname` — Kernel information

```bash
uname -a
```

Displays system and kernel information.

```bash
uname -r
```

Displays the kernel release.

**DevOps use:** Confirm the kernel version when investigating driver compatibility, kernel updates, or system-level issues.

## 3.2 `hostname` — Machine name

```bash
hostname
```

Display the system hostname.

```bash
hostnamectl
```

On systemd-based systems, this shows hostname and related operating system information.

## 3.3 `cat /etc/os-release` — Operating system details

```bash
cat /etc/os-release
```

Displays the Linux distribution name and version.

This is useful when checking whether a server runs Ubuntu, RHEL, Amazon Linux, or another distribution.

## 3.4 `uptime` — System uptime

```bash
uptime
```

Shows how long the system has been running, the number of logged-in users, and load averages.

Example:

```text
10:30:00 up 5 days, 2 users, load average: 0.50, 0.75, 0.80
```

The load averages generally represent the system's average load over 1, 5, and 15 minutes. Load is not the same as CPU utilization; it can also include tasks waiting for certain resources.

## 3.5 `date` — Current date and time

```bash
date
```

Display the current system date and time.

```bash
date '+%Y-%m-%d %H:%M:%S'
```

Display a formatted timestamp.

## 3.6 `timedatectl` — Time configuration

```bash
timedatectl
```

Shows time, timezone, and clock synchronization information on systems that support it.

**DevOps use:** Investigate timestamp differences between application logs and monitoring systems.

## 3.7 `lscpu` — CPU information

```bash
lscpu
```

Shows CPU architecture, logical CPU count, and other processor information.

## 3.8 `lsblk` — Block devices

```bash
lsblk
```

Displays disks, partitions, and their relationships.

```bash
lsblk -f
```

Also displays filesystem types, labels, and UUIDs where available.

## 3.9 `lspci` — PCI devices

```bash
lspci
```

Lists PCI devices, such as certain network and storage controllers.

The command may require installing the `pciutils` package.

## 3.10 `lsusb` — USB devices

```bash
lsusb
```

Lists USB devices. It is useful when diagnosing systems that use USB-connected hardware.

---

# 4. Process Management

A process is a running instance of a program.

When a Linux server runs a web application, database, or monitoring agent, each running program may have one or more processes.

## 4.1 `ps` — View processes

```bash
ps
```

Displays processes associated with the current terminal.

```bash
ps aux
```

Displays a detailed snapshot of many processes.

```bash
ps -ef
```

Displays processes in another commonly used detailed format.

### Find a specific process

```bash
ps aux | grep nginx
```

This may also display the `grep` command itself.

A more convenient alternative on many Linux systems is:

```bash
pgrep -a nginx
```

It lists matching process IDs and command lines.

## 4.2 `top` — Live process monitoring

```bash
top
```

Shows processes and system activity in a continuously updated display.

Useful keys inside `top` include:

- `P`: sort by CPU usage.
- `M`: sort by memory usage.
- `q`: quit.

Available keys can vary slightly by implementation.

## 4.3 `htop` — Interactive process viewer

```bash
htop
```

Provides an interactive view of processes, CPU, and memory.

It may not be installed by default.

On Ubuntu:

```bash
sudo apt update
sudo apt install htop
```

## 4.4 `pgrep` — Find process IDs

```bash
pgrep nginx
```

Displays IDs of processes whose names match `nginx`.

```bash
pgrep -a nginx
```

Also shows command lines.

## 4.5 `pstree` — Process tree

```bash
pstree
```

Displays processes in a tree structure.

```bash
pstree -p
```

Includes process IDs.

Useful for understanding which process launched another process.

## 4.6 `kill` — Send a signal to a process

```bash
kill PID
```

Normally sends `SIGTERM`, requesting that the process terminate gracefully.

Example:

```bash
kill 1234
```

Replace `1234` with the actual process ID.

To request termination explicitly:

```bash
kill -TERM 1234
```

To force termination:

```bash
kill -KILL 1234
```

Use `SIGKILL` only when necessary. The process cannot handle this signal to perform normal cleanup.

**Safety rule:** Always verify the PID and process identity before sending signals.

## 4.7 `pkill` — Signal processes by name

```bash
pkill -TERM process_name
```

Sends a termination signal to processes matching the supplied name.

Use caution because a pattern may match multiple processes.

## 4.8 `nice` and `renice` — Process scheduling priority

```bash
nice -n 10 command
```

Runs a command with a modified nice value.

```bash
renice 10 -p PID
```

Changes the nice value of a running process.

A higher nice value generally means lower scheduling priority among normal-priority processes.

These commands affect CPU scheduling priority, not disk priority.

## 4.9 `jobs`, `bg`, `fg` and `nohup`

These are useful for commands started from a shell.

```bash
sleep 300 &
```

Starts a command in the background.

```bash
jobs
```

Lists jobs managed by the current shell.

```bash
fg
```

Brings a background or stopped job to the foreground.

```bash
bg
```

Continues a stopped job in the background.

```bash
nohup long_running_command > output.log 2>&1 &
```

Runs a command so that it ignores the hangup signal by default, with output redirected to a file.

`nohup` does not replace a service manager. For long-running production applications, systemd or an appropriate process supervisor is usually better.

---

# 5. CPU, Memory and Performance

## 5.1 `free` — Memory usage

```bash
free -h
```

Shows total, used, free, shared, buffer/cache, and available memory.

**Important:** Linux uses otherwise available memory for caching. Low `free` memory alone does not mean the system is out of memory. The `available` value is often more useful for estimating memory available to applications.

## 5.2 `vmstat` — Virtual memory and system statistics

```bash
vmstat 1 5
```

Displays system statistics every second, five times.

It can report process activity, memory, paging, block I/O, and CPU statistics.

The first report often includes averages since boot, so pay attention to the later interval reports.

## 5.3 `iostat` — CPU and disk I/O

```bash
iostat -xz 1 5
```

Provides CPU and device I/O statistics at one-second intervals for five reports.

It is commonly supplied by the `sysstat` package.

On Ubuntu:

```bash
sudo apt install sysstat
```

Useful when investigating slow disks or high storage latency.

## 5.4 `mpstat` — CPU statistics

```bash
mpstat -P ALL 1 5
```

Displays CPU statistics, potentially for each processor.

Also commonly supplied by `sysstat`.

## 5.5 `sar` — Historical system activity

```bash
sar -u 1 5
```

Displays CPU activity at intervals when supported and configured.

`sar` can also help review historical performance data if system activity collection is enabled.

## 5.6 `watch` — Repeat a command

```bash
watch -n 2 df -h
```

Refreshes the disk usage display every two seconds.

Another example:

```bash
watch -n 2 free -h
```

Press `Ctrl+C` to stop it.

Use `watch` for quick observation. For long-term monitoring and alerting, use a monitoring solution.

---

# 6. Disk and Filesystem Management

Disk-related commands help identify storage capacity issues, large directories, mounted filesystems, and inodes.

## 6.1 `df` — Filesystem capacity

```bash
df -h
```

Displays filesystem capacity in human-readable units.

```bash
df -h /
```

Checks the filesystem containing the root directory.

```bash
df -i
```

Shows inode usage.

**Important:** A filesystem can run out of inodes even if it still has free disk space. This can prevent creation of new files.

## 6.2 `du` — Directory disk usage

```bash
du -sh /var/log
```

Displays the total apparent disk usage reported by `du` for the specified directory, in human-readable form.

```bash
du -h --max-depth=1 /var
```

On GNU systems, shows usage for immediate subdirectories.

## 6.3 Find the largest directories

```bash
sudo du -xhd1 /var 2>/dev/null | sort -h
```

Explanation:

- `du`: estimates directory usage.
- `-x`: stay on the same filesystem.
- `-h`: human-readable sizes.
- `-d1`: limit directory depth to one.
- `sort -h`: sort sizes in human-readable order.

This command may need `sudo` to inspect protected directories.

## 6.4 `findmnt` — Mounted filesystems

```bash
findmnt
```

Shows mounted filesystems and their mount points.

```bash
findmnt /
```

Shows the filesystem associated with the root mount.

## 6.5 `mount` — View or mount filesystems

```bash
mount
```

Displays currently mounted filesystems.

Mounting or unmounting filesystems changes system state and can disrupt applications. Do not experiment with these operations on production storage.

## 6.6 `blkid` — Filesystem identification

```bash
sudo blkid
```

Displays filesystem types, UUIDs, and labels for recognized block devices.

Useful when troubleshooting filesystem identification or persistent mount configuration.

## 6.7 `fdisk` — Partition information

```bash
sudo fdisk -l
```

Lists disk partition tables on supported systems.

Do not use partition-editing commands on a disk until you understand its layout and have verified the target device. Incorrect partition changes can destroy data.

## 6.8 `du` versus `df`

| Command | Purpose |
|---|---|
| `df -h` | Filesystem-level space availability |
| `du -sh directory` | Space attributed to files in a directory tree |
| `df -i` | Inode availability |
| `lsblk` | Disk and partition layout |

If `df` shows a full filesystem but `du` cannot explain the usage, investigate deleted files still held open by processes, filesystem metadata, reserved space, and mount boundaries.

## 6.9 `lsof` — Open files

```bash
sudo lsof
```

Lists open files and related process information.

Find processes holding a particular file:

```bash
sudo lsof /var/log/syslog
```

Investigate deleted files still held open:

```bash
sudo lsof +L1
```

This can help explain why disk space has not been released after a large file was deleted.

Do not kill processes indiscriminately. Identify the application and choose an appropriate, safe recovery method.

---

# 7. File Searching and Text Processing

These commands are particularly important for log analysis, audits, troubleshooting, and automation.

## 7.1 `find` — Search files

```bash
find /var/log -type f -name "*.log"
```

Find regular files ending in `.log`.

```bash
find . -type f -mtime -1
```

Find regular files modified within approximately the last 24 hours.

```bash
find . -type f -size +100M
```

Find regular files larger than 100 MiB according to GNU `find` size semantics.

```bash
find . -type f -empty
```

Find empty regular files.

### Search by permission

```bash
find . -type f -perm /111
```

On GNU `find`, finds regular files with at least one execute bit set.

## 7.2 `grep` — Search text

```bash
grep "ERROR" application.log
```

Search for lines containing `ERROR`.

```bash
grep -in "error" application.log
```

Search case-insensitively and show line numbers.

```bash
grep -Rin "error" /var/log/myapp
```

Recursively search a directory.

```bash
grep -E "ERROR|FATAL|CRITICAL" application.log
```

Use extended regular expressions to match any of the supplied patterns.

## 7.3 `awk` — Process text by fields

```bash
awk '{print $1}' access.log
```

Prints the first whitespace-separated field from each line.

For a colon-separated file:

```bash
awk -F: '{print $1}' /etc/passwd
```

Prints the first colon-separated field, commonly the username.

Count lines containing `ERROR`:

```bash
awk '/ERROR/ {count++} END {print count+0}' application.log
```

## 7.4 `sed` — Transform text

```bash
sed 's/old/new/g' file.txt
```

Prints output with substitutions applied.

To view matching lines with line numbers:

```bash
sed -n '/ERROR/=' application.log
```

The `=` command prints line numbers for matching lines.

Use in-place editing carefully. Always make a backup before changing important configuration files.

## 7.5 `sort` and `uniq`

```bash
sort names.txt
```

Sort lines.

```bash
sort names.txt | uniq
```

Remove adjacent duplicate lines after sorting.

```bash
sort names.txt | uniq -c
```

Count repeated lines.

## 7.6 `cut` — Select fields

```bash
cut -d: -f1 /etc/passwd
```

Select the first colon-separated field.

```bash
cut -d, -f2 data.csv
```

Select the second comma-delimited field when the input is simple and does not contain quoted commas. For complex CSV data, use a CSV-aware tool.

## 7.7 `xargs` — Build command arguments

```bash
printf '%s\n' one two three | xargs -n1 echo
```

Runs the command once for each input item.

For filenames containing whitespace or newlines, use null-delimited input when the producing command supports it:

```bash
find . -type f -print0 | xargs -0 -r file
```

This is safer for unusual filenames.

## 7.8 `tee` — Display and save output

```bash
df -h | tee disk-report.txt
```

Displays the result and writes it to a file.

```bash
df -h | tee -a disk-report.txt
```

Appends the result to the file.

## 7.9 `diff` — Compare files

```bash
diff old.conf new.conf
```

Shows line differences between two files.

Useful for checking configuration changes.

## 7.10 `wc` — Count lines, words and bytes

```bash
wc -l application.log
wc -w notes.txt
wc -c notes.txt
```

These count lines, words, and bytes.

---

# 8. Logs and Troubleshooting

Linux logs help you understand what happened before, during, and after a failure.

## 8.1 `journalctl` — Read systemd journal logs

```bash
journalctl
```

Displays available journal entries.

```bash
journalctl -b
```

Shows logs from the current boot.

```bash
journalctl -b -1
```

Shows logs from the previous boot if they were retained.

```bash
journalctl -u ssh --since "1 hour ago"
```

Shows recent logs for the `ssh` unit on systems where that is the service name.

```bash
journalctl -p err -b
```

Shows error-priority messages from the current boot.

```bash
journalctl -f
```

Follows new journal entries.

Use `Ctrl+C` to stop following logs.

## 8.2 `dmesg` — Kernel messages

```bash
dmesg
```

Displays kernel ring buffer messages.

```bash
dmesg -T
```

Attempts to show human-readable timestamps on supported systems.

Useful for investigating device, driver, hardware, and kernel-level issues.

Access may be restricted by system security settings.

## 8.3 `tail` — Inspect recent log lines

```bash
tail -n 100 application.log
```

Displays the last 100 lines.

```bash
tail -f application.log
```

Follows a log as new lines are appended.

## 8.4 `head` — Inspect the beginning of a file

```bash
head -n 20 application.log
```

Displays the first 20 lines.

## 8.5 Common log locations

| Location | Typical purpose |
|---|---|
| `/var/log/syslog` | General system messages on Ubuntu |
| `/var/log/auth.log` | Authentication-related logs on Ubuntu |
| `/var/log/messages` | General system messages on many RHEL-based systems |
| `/var/log/secure` | Authentication-related logs on many RHEL-based systems |
| `/var/log/kern.log` | Kernel-related log messages on some Ubuntu systems |
| `/var/log/nginx/` | Common NGINX log directory |
| `/var/log/apache2/` | Common Apache log directory on Ubuntu |

Exact paths depend on the distribution and logging configuration. Many system logs are also available through `journalctl`.

## 8.6 Example: Find recent errors

```bash
journalctl -p err --since "30 minutes ago"
```

For an application log:

```bash
grep -inE "ERROR|FATAL|CRITICAL" application.log | tail -n 50
```

**DevOps habit:** Record the error timestamp, identify the affected component, and compare application logs with service and system logs.

---

# 9. Networking Commands

Networking commands help investigate connectivity, IP configuration, listening ports, DNS resolution, and routes.

## 9.1 `ip addr` — IP addresses

```bash
ip addr
```

Displays network interfaces and addresses.

Short form:

```bash
ip a
```

## 9.2 `ip route` — Routing table

```bash
ip route
```

Displays routing information, including the default route where configured.

Useful when investigating whether a server has an appropriate route to another network.

## 9.3 `ping` — Basic reachability test

```bash
ping -c 4 8.8.8.8
```

Sends four ICMP echo requests.

A failed ping does not always mean the destination is unavailable; firewalls may block ICMP.

## 9.4 `ss` — Sockets and listening ports

```bash
ss -tuln
```

Displays TCP and UDP sockets, including listening sockets, without resolving names.

```bash
sudo ss -tulpn
```

Also requests process information where permissions permit.

Useful when investigating whether an application is listening on the expected port.

## 9.5 `curl` — Test HTTP and APIs

```bash
curl -I https://example.com
```

Requests response headers using an HTTP HEAD request when supported by the server.

```bash
curl -sS https://example.com
```

Displays response content without the progress meter while still reporting errors.

For status and timing information:

```bash
curl -sS -o /dev/null -w 'HTTP status: %{http_code}\n' https://example.com
```

Do not send credentials or sensitive data to untrusted endpoints.

## 9.6 `wget` — Retrieve files

```bash
wget https://example.com/file.txt
```

Downloads a file from a URL.

Check the URL and trustworthiness of downloaded files before executing them.

## 9.7 `dig` — DNS lookup

```bash
dig example.com
```

Queries DNS and displays the response.

```bash
dig +short example.com
```

Displays concise answer data.

`dig` may require installation of a DNS utilities package.

## 9.8 `getent` — System name resolution

```bash
getent hosts example.com
```

Looks up host information using the system's configured name-service sources.

This can be useful when investigating differences between direct DNS queries and the system resolver.

## 9.9 `traceroute` or `tracepath`

```bash
traceroute example.com
```

Attempts to show network hops towards a destination.

On systems without `traceroute`, `tracepath` may be available:

```bash
tracepath example.com
```

These tools can be affected by filtering, routing policy, and network configuration.

## 9.10 `nc` — Test TCP connectivity

```bash
nc -vz example.com 443
```

Attempts a TCP connection to port 443.

This is useful for testing whether a port is reachable from the current machine. A successful connection does not prove that the application is healthy.

## 9.11 `ssh` — Remote login

```bash
ssh username@server-address
```

Connects to a remote server.

For an EC2 instance with a private key:

```bash
ssh -i /path/to/key.pem ubuntu@EC2_PUBLIC_IP
```

The username depends on the AMI. Protect the private key and verify host identity.

## 9.12 `scp` — Copy files over SSH

```bash
scp report.txt username@server-address:/tmp/
```

Copies a local file to a remote server.

To copy from a remote server to the local machine:

```bash
scp username@server-address:/tmp/report.txt .
```

---

# 10. User, Permission and Security Commands

## 10.1 `id` — User and group IDs

```bash
id
```

Displays your UID, primary GID, and group memberships.

```bash
id username
```

Displays identity information for another user.

## 10.2 `who` and `w`

```bash
who
```

Shows logged-in sessions.

```bash
w
```

Displays logged-in users and their activity information.

## 10.3 `sudo` — Run an authorized command with elevated privileges

```bash
sudo command
```

Example:

```bash
sudo systemctl status ssh
```

Use `sudo` only when needed and only for commands you understand.

## 10.4 `chmod` — Change permissions

```bash
chmod 640 file.txt
```

Sets permissions to:

- Owner: read and write.
- Group: read.
- Others: no access.

For a script:

```bash
chmod 750 script.sh
```

## 10.5 `chown` — Change ownership

```bash
sudo chown username:groupname file.txt
```

Changes the file owner and group.

Verify the names and target path before changing ownership.

## 10.6 `umask` — Default permission mask

```bash
umask
```

Displays the current mask.

```bash
umask 027
```

Sets the mask for the current shell and its child processes.

Actual default permissions depend on the mask and the permissions requested by the creating program.

## 10.7 `getfacl` and `setfacl` — Access control lists

```bash
getfacl file.txt
```

Displays ACL permissions when ACL tools and filesystem support are available.

```bash
setfacl -m u:username:r file.txt
```

Adds read permission for the specified user in the file's ACL.

Be careful when changing ACLs on production files.

## 10.8 `sudo -l` — Inspect sudo permissions

```bash
sudo -l
```

Shows commands the current user is permitted to run through sudo, subject to system policy.

## 10.9 `last` — Login history

```bash
last
```

Displays login history from available accounting records.

The completeness of the output depends on system configuration and retained records.

## 10.10 `ss` and `lsof` for security investigation

```bash
sudo ss -tulpn
```

Inspect listening ports.

```bash
sudo lsof -i
```

Inspect network-related open files and sockets.

Use these commands as part of a broader security review rather than treating any one output as proof that a system is secure.

---

# 11. Services and System Administration

## 11.1 `systemctl status`

```bash
systemctl status ssh
```

Displays service status and recent log information when available.

## 11.2 Start and stop services

```bash
sudo systemctl start SERVICE_NAME
```

Starts a service.

```bash
sudo systemctl stop SERVICE_NAME
```

Stops a service.

```bash
sudo systemctl restart SERVICE_NAME
```

Restarts a service.

```bash
sudo systemctl reload SERVICE_NAME
```

Requests a configuration reload when the service supports it.

Never restart or stop an important production service without understanding the impact and following the appropriate change procedure.

## 11.3 Enable or disable automatic startup

```bash
sudo systemctl enable SERVICE_NAME
```

Configures a service to start automatically according to its unit configuration.

```bash
sudo systemctl disable SERVICE_NAME
```

Disables that automatic-start configuration.

These operations are distinct from starting or stopping the service now.

## 11.4 Check service state

```bash
systemctl is-active SERVICE_NAME
```

Checks the current active state.

```bash
systemctl is-enabled SERVICE_NAME
```

Checks whether the service is enabled for automatic startup.

## 11.5 List failed services

```bash
systemctl --failed
```

Displays failed systemd units.

## 11.6 `journalctl -u`

```bash
journalctl -u SERVICE_NAME --since today
```

Displays journal entries for the specified unit since the beginning of the current day, subject to journal availability and retention.

---

# 12. Archives and Compression

## 12.1 `tar` — Create an archive

```bash
tar -cvf backup.tar directory/
```

Creates an uncompressed archive.

```bash
tar -czf backup.tar.gz directory/
```

Creates a gzip-compressed archive.

## 12.2 List archive contents

```bash
tar -tzf backup.tar.gz
```

Lists files inside the compressed archive.

## 12.3 Extract an archive

```bash
tar -xzf backup.tar.gz
```

Extracts the archive into the current directory.

Before extracting an unfamiliar archive, inspect its contents and paths. Extraction can overwrite files or write into unexpected locations if the archive is unsafe.

## 12.4 `gzip` and `gunzip`

```bash
gzip file.txt
```

Compresses the file into a gzip file and ordinarily removes the original input file after successful compression.

```bash
gunzip file.txt.gz
```

Decompresses the gzip file.

## 12.5 `zip` and `unzip`

```bash
zip -r backup.zip directory/
```

Creates a ZIP archive.

```bash
unzip backup.zip
```

Extracts a ZIP archive.

These commands may need to be installed separately.

---

# 13. SSH and Remote Administration

SSH is widely used to manage remote Linux servers securely.

## 13.1 Generate an SSH key pair

```bash
ssh-keygen -t ed25519
```

Creates an Ed25519 key pair on systems supporting it.

Protect the private key. Do not share it or upload it to a public repository.

## 13.2 Copy a public key to a server

```bash
ssh-copy-id username@server-address
```

This installs the public key for authentication if the target server permits the operation.

## 13.3 Use SSH configuration

A client configuration file is commonly located at:

```text
~/.ssh/config
```

Example:

```text
Host dev-server
    HostName 192.0.2.10
    User ubuntu
    IdentityFile ~/.ssh/dev-server-key
```

The address is an example and must be replaced with the correct server address.

Connect with:

```bash
ssh dev-server
```

## 13.4 Copy a directory

```bash
scp -r project/ username@server-address:/tmp/
```

Copies a directory recursively.

## 13.5 `rsync` — Efficient file synchronization

```bash
rsync -av project/ destination/
```

Synchronizes directory contents locally or with a supported remote destination.

Example:

```bash
rsync -av project/ username@server-address:/tmp/project/
```

The trailing slash on `project/` means to copy its contents into the destination directory.

**Important:** `rsync` options can delete or overwrite data. Understand options such as `--delete` before using them.

---

# 14. Useful Command Combinations

Real troubleshooting often requires combining commands.

## 14.1 Find the largest directories under `/var`

```bash
sudo du -xhd1 /var 2>/dev/null | sort -h
```

## 14.2 Find the largest regular files in a directory

```bash
find /var/log -type f -printf '%s %p\n' 2>/dev/null | sort -n | tail -n 10
```

This lists up to ten regular files with the largest reported byte sizes, where GNU `find` is available.

## 14.3 Find errors in recent application logs

```bash
grep -inE "ERROR|FATAL|CRITICAL" application.log | tail -n 50
```

## 14.4 Check listening TCP ports

```bash
ss -ltn
```

## 14.5 Check disk and memory together

```bash
df -h
free -h
```

## 14.6 Search for recently modified files

```bash
find /var/log -type f -mmin -60
```

Finds regular files modified within approximately the last 60 minutes.

## 14.7 Check which process is using a port

```bash
sudo ss -ltnp
```

Look for the required port in the output.

## 14.8 Inspect a service after failure

```bash
systemctl status SERVICE_NAME
journalctl -u SERVICE_NAME -b --no-pager | tail -n 100
```

Replace `SERVICE_NAME` with the actual systemd unit name.

## 14.9 Count error lines

```bash
grep -ic "ERROR" application.log
```

This counts matching lines, not necessarily the total number of occurrences if a line contains `ERROR` multiple times.

## 14.10 Compare configuration files

```bash
diff -u old.conf new.conf
```

Shows a unified diff of the two files.

---

# 15. Advanced Command Safety

Linux commands can affect files, services, networks, and entire servers. Follow these practices.

1. Understand a command before running it with `sudo`.
2. Check the target path before deleting or modifying files.
3. Avoid blindly copying destructive commands from the internet.
4. Test scripts with sample data.
5. Make a backup before changing important configuration.
6. Do not kill a process without verifying its identity and purpose.
7. Avoid exposing credentials in command-line arguments, logs, or shell history.
8. Use appropriate permissions for scripts, SSH keys, and reports.
9. Use `--` with commands that support it when passing potentially ambiguous paths.
10. Avoid running downloaded scripts directly without reviewing their contents and verifying their source.
11. Be careful with `find -exec`, `xargs`, recursive permissions, and recursive deletion.
12. Use monitoring tools for long-term trends rather than relying only on a one-time command.
13. Verify the target host before running administrative operations over SSH.
14. Prefer a tested backup-and-restore procedure before making high-risk storage changes.

### A useful habit

Before a risky operation, ask yourself:

- What will this command change?
- Which machine am I connected to?
- Which user will run it?
- Can the operation be reversed?
- Have I verified the exact target?
- Is there a tested recovery path?

---

# 16. Mini Project: Linux Server Diagnostic Report

## Project objective

Create a Bash script that collects important Linux server information into a timestamped report.

This project combines advanced Linux commands with shell scripting.

## What will the script check?

- Hostname and operating system.
- Current date and uptime.
- CPU information.
- Memory usage.
- Filesystem usage.
- Top processes by CPU.
- Listening TCP ports.
- Failed systemd units.
- Available disk space and memory summary.

The project generates a report without intentionally modifying services or system configuration.

## Step 1: Create the project directory

```bash
mkdir -p ~/linux-diagnostic-project/reports
cd ~/linux-diagnostic-project
```

## Step 2: Create the script

Create a file named `linux_diagnostic.sh`:

```bash
nano linux_diagnostic.sh
```

Paste the following script:

```bash
#!/usr/bin/env bash
set -uo pipefail

REPORT_DIR="$HOME/linux-diagnostic-project/reports"
TIMESTAMP="$(date '+%Y%m%d_%H%M%S')"
REPORT_FILE="$REPORT_DIR/diagnostic_$TIMESTAMP.txt"

mkdir -p -- "$REPORT_DIR"

if ! touch "$REPORT_FILE"; then
    echo "ERROR: Cannot create report file." >&2
    exit 1
fi

section() {
    printf '\n===== %s =====\n' "$1"
}

run_check() {
    local title="$1"
    shift

    section "$title"

    if "$@"; then
        :
    else
        local status=$?
        printf 'WARNING: Command failed with status %s\n' "$status"
    fi
}

{
    section "LINUX SERVER DIAGNOSTIC REPORT"
    echo "Generated: $(date)"
    echo "Hostname: $(hostname)"
    echo "User: $(whoami)"

    section "OPERATING SYSTEM"
    if [[ -r /etc/os-release ]]; then
        cat /etc/os-release
    else
        echo "Operating system information is unavailable."
    fi

    section "KERNEL"
    uname -a

    section "UPTIME"
    uptime

    section "CPU INFORMATION"
    if command -v lscpu >/dev/null 2>&1; then
        lscpu
    else
        echo "lscpu is not installed."
    fi

    section "MEMORY"
    if command -v free >/dev/null 2>&1; then
        free -h
    else
        echo "free command is unavailable."
    fi

    section "FILESYSTEM CAPACITY"
    df -h

    section "INODE USAGE"
    df -i

    section "TOP PROCESSES BY CPU"
    ps -eo pid,ppid,comm,%cpu,%mem --sort=-%cpu | head -n 11

    section "LISTENING TCP PORTS"
    if command -v ss >/dev/null 2>&1; then
        ss -ltn
    else
        echo "ss command is unavailable."
    fi

    section "FAILED SYSTEMD UNITS"
    if command -v systemctl >/dev/null 2>&1; then
        systemctl --failed --no-pager
    else
        echo "systemctl is unavailable."
    fi

    section "END OF REPORT"
    echo "Report generated at: $(date)"
} > "$REPORT_FILE" 2>&1

echo "Diagnostic report created:"
echo "$REPORT_FILE"
```

## Step 3: Check the script

```bash
bash -n linux_diagnostic.sh
```

If there is no output, the script passed Bash's syntax check.

## Step 4: Run the script

```bash
chmod +x linux_diagnostic.sh
./linux_diagnostic.sh
```

## Step 5: Read the report

```bash
ls -lh reports
```

Open the latest report using its actual filename:

```bash
less reports/diagnostic_YYYYMMDD_HHMMSS.txt
```

Replace the example filename with the one created by the script.

## Step 6: Extend the project

Try adding these improvements:

- [ ] Warn when a filesystem reaches a configured usage threshold.
- [ ] Include the five largest files in a selected log directory.
- [ ] Add a command-line argument to select the output directory.
- [ ] Check whether a specified service is active.
- [ ] Include a section for recent system errors.
- [ ] Add an optional summary at the end.
- [ ] Run ShellCheck against the script.
- [ ] Document the required commands and packages.
- [ ] Test the script on Ubuntu and another Linux distribution.
- [ ] Add a README with setup instructions and sample report output.

## Project limitations

This project provides a diagnostic snapshot, not continuous monitoring.

Some commands may be unavailable or produce limited output depending on the distribution, permissions, and system configuration. The script also does not automatically fix problems.

For production use, consider report retention, access controls, error reporting, performance overhead, and the handling of sensitive information.

---

# 17. Hands-On Practice Labs

Complete these exercises on a test system.

## Lab 1: System information

- [ ] Identify the operating system.
- [ ] Find the kernel version.
- [ ] Check uptime.
- [ ] Check CPU details.
- [ ] Identify block devices and mounted filesystems.

Commands to practise:

```bash
cat /etc/os-release
uname -r
uptime
lscpu
lsblk -f
findmnt
```

## Lab 2: Process troubleshooting

- [ ] Display running processes.
- [ ] Find a process by name.
- [ ] View CPU and memory usage.
- [ ] Display the process tree.
- [ ] Explain the difference between `SIGTERM` and `SIGKILL`.

## Lab 3: Disk troubleshooting

- [ ] Check filesystem capacity.
- [ ] Check inode usage.
- [ ] Identify the largest directories in a practice folder.
- [ ] Find files larger than a chosen size.
- [ ] Explain why `df` and `du` can show different results.

## Lab 4: Log analysis

- [ ] Search for `ERROR`.
- [ ] Display matching line numbers.
- [ ] Count matching lines.
- [ ] Follow a log file.
- [ ] Find recent system errors using `journalctl`.

## Lab 5: Network troubleshooting

- [ ] Inspect IP addresses.
- [ ] View the routing table.
- [ ] Check DNS resolution.
- [ ] Check listening ports.
- [ ] Test an HTTP endpoint using `curl`.
- [ ] Test a TCP port using `nc`, if installed.

## Lab 6: Service troubleshooting

- [ ] Check a service's current state.
- [ ] Check whether it is enabled.
- [ ] View recent service logs.
- [ ] List failed systemd units.
- [ ] Explain why a service may be enabled but not active.

## Lab 7: Complete the diagnostic project

- [ ] Run the script.
- [ ] Inspect the generated report.
- [ ] Test a failure case.
- [ ] Add a new check.
- [ ] Run `bash -n`.
- [ ] Run ShellCheck.
- [ ] Write down what each section of the report tells you.

---

# 18. Troubleshooting Scenarios

## Scenario 1: The server is slow

Start with:

```bash
uptime
top
free -h
vmstat 1 5
```

If disk I/O appears to be a problem and `iostat` is available:

```bash
iostat -xz 1 5
```

Interpret the output together. A high load average, high CPU usage, memory pressure, and storage latency are different problems and require different investigation.

## Scenario 2: The disk is full

Run:

```bash
df -h
df -i
sudo du -xhd1 /var 2>/dev/null | sort -h
sudo lsof +L1
```

Investigate both block-space usage and inode usage. Do not immediately delete logs or files without determining their purpose and retention requirements.

## Scenario 3: A service is not working

Run:

```bash
systemctl status SERVICE_NAME
journalctl -u SERVICE_NAME -b --no-pager | tail -n 100
```

Check configuration, dependencies, permissions, ports, and application-specific logs.

## Scenario 4: The application is unreachable

Run:

```bash
ip addr
ip route
ss -ltn
```

Then test the application locally where appropriate:

```bash
curl -I http://127.0.0.1:PORT/
```

Replace `PORT` with the actual application port.

If the application works locally but not remotely, investigate firewall rules, cloud security groups, routing, DNS, and the network path.

## Scenario 5: A port is already in use

Check:

```bash
sudo ss -ltnp
```

Identify the process using the relevant port. Do not terminate it until you understand why it is running and whether another service depends on it.

## Scenario 6: A log contains no errors

Try:

```bash
grep -inE "ERROR|FATAL|CRITICAL|Exception|Failed" application.log
```

A lack of matches does not prove that the application is healthy. Check the expected log format, time range, application state, and relevant monitoring data.

## Scenario 7: A process disappeared after deleting a file

Check:

```bash
sudo lsof +L1
df -h
```

A deleted file may still consume space while a process holds it open. Follow the application's documented procedure to release the file handle safely.

---

# 19. Linux Interview Questions and Answers

## Beginner and fundamentals

### 1. What is the difference between Linux and a Linux distribution?

Linux technically refers to the kernel. A Linux distribution combines the kernel with system tools, package management, libraries, and other software to provide a usable operating system.

### 2. What is a process?

A process is a running instance of a program with associated execution state and resources.

### 3. What is a PID?

PID means Process ID. It is an identifier assigned to a process.

### 4. What is the difference between `ps` and `top`?

`ps` provides a snapshot of process information. `top` provides a continuously updating interactive view of system and process activity.

### 5. What is the difference between `kill` and `kill -9`?

`kill` normally sends `SIGTERM`, requesting graceful termination. `kill -9` sends `SIGKILL`, which cannot be caught or handled by the target process.

### 6. What is load average?

Load average represents average system load over time. On Linux it includes runnable tasks and tasks in certain uninterruptible states, so it should not be interpreted as CPU percentage alone.

### 7. What is the difference between RAM and disk storage?

RAM is volatile working memory used by running programs. Disk storage is persistent storage used for files and other data.

### 8. What is a kernel?

The kernel is the core of the operating system. It manages resources such as processes, memory, devices, and system calls.

## Disk and filesystem

### 9. What is the difference between `df` and `du`?

`df` reports filesystem space usage and availability. `du` estimates space attributed to files and directories.

### 10. What is an inode?

An inode stores filesystem metadata about a file, such as ownership, permissions, timestamps, and references to data blocks. Filenames are stored in directory entries that reference inodes.

### 11. What happens when a filesystem runs out of inodes?

Creating new files can fail even if some disk space remains available.

### 12. How do you find large files?

For example:

```bash
find /var/log -type f -size +100M
```

This searches for regular files larger than 100 MiB according to GNU `find` size semantics.

### 13. Why might `df` show a full disk when `du` does not?

Possible causes include deleted files still held open by processes, filesystem metadata, reserved blocks, different mount boundaries, or files inaccessible to the user running `du`.

### 14. What does `lsblk` do?

It displays block devices and their relationships, including disks and partitions.

### 15. What does `findmnt` do?

It displays mounted filesystems and their mount points.

## Processes and performance

### 16. How do you find a process by name?

Use:

```bash
pgrep -a process_name
```

or inspect a process listing with `ps`.

### 17. How do you identify the process using a port?

Use:

```bash
sudo ss -ltnp
```

Look for the required port and the associated process information.

### 18. What is the difference between `SIGTERM` and `SIGKILL`?

`SIGTERM` requests termination and can be handled by the process. `SIGKILL` forcibly terminates the process and cannot be handled by it.

### 19. What does `nice` do?

It changes the scheduling nice value of a process, influencing its relative CPU scheduling priority.

### 20. What does `vmstat` help diagnose?

It provides statistics about processes, memory, paging, block I/O, and CPU activity. Repeated interval reports can help investigate performance issues.

### 21. What does `iostat` do?

It reports CPU and storage I/O statistics and can help investigate disk performance.

### 22. What is the purpose of `lsof`?

It lists open files and related process information. It can help identify which process is using a file or network socket.

## Networking

### 23. What does `ip addr` show?

It shows network interfaces and their IP addresses.

### 24. What does `ip route` show?

It displays the system routing table.

### 25. What is the difference between `ping` and `curl`?

`ping` tests ICMP echo reachability where permitted. `curl` can communicate with HTTP and other supported application protocols.

### 26. What is the purpose of `ss`?

It displays socket information, including listening and established connections.

### 27. What is DNS?

DNS translates names, such as `example.com`, into DNS records that can include IP addresses and other information.

### 28. How do you test DNS resolution?

For example:

```bash
dig +short example.com
```

or:

```bash
getent hosts example.com
```

The results can differ because the commands may use different lookup paths.

### 29. What does `nc -vz host port` do?

It attempts a connection to the specified host and port. It can help determine whether a TCP connection is possible from the current system.

## Logs and services

### 30. What is the purpose of `journalctl`?

It reads entries from the systemd journal, including system and service logs.

### 31. How do you check failed systemd units?

Use:

```bash
systemctl --failed
```

### 32. What is the difference between `systemctl start` and `systemctl enable`?

`start` requests that the service run now. `enable` configures it to start automatically according to its unit configuration.

### 33. How do you check service logs?

Use:

```bash
journalctl -u SERVICE_NAME
```

Replace `SERVICE_NAME` with the actual systemd unit name.

### 34. What is `dmesg` used for?

It displays kernel ring buffer messages and can help investigate kernel and device-related events.

## Text processing and automation

### 35. What is the difference between `grep`, `awk`, and `sed`?

- `grep`: searches text for patterns.
- `awk`: processes text by fields, patterns, and actions.
- `sed`: transforms or filters text streams.

### 36. What is a pipe?

A pipe (`|`) sends one command's standard output to another command's standard input.

### 37. What is the difference between `>` and `>>`?

`>` overwrites or creates a file. `>>` appends to it.

### 38. What does `tee` do?

It sends input to standard output and writes a copy to one or more files.

### 39. What is `rsync` used for?

It synchronizes files and directories efficiently, locally or between systems over supported remote transports such as SSH.

### 40. What is the purpose of `cron`?

Cron schedules commands to run at specified times on systems configured to use cron.

## DevOps scenario questions

### 41. How would you troubleshoot a Linux server with high CPU usage?

Check `top`, `ps`, and CPU statistics. Identify the processes consuming CPU, correlate them with application activity and logs, and determine whether the load is expected before taking action.

### 42. How would you troubleshoot a full filesystem?

Check `df -h` and `df -i`, identify large directories using `du`, investigate deleted files held open using `lsof`, and review retention policies before cleaning anything.

### 43. How would you troubleshoot a service that will not start?

Use `systemctl status`, inspect `journalctl -u`, validate configuration, check permissions and dependencies, and verify that required ports are available.

### 44. How would you investigate a website that is not reachable?

Check the service state, listening ports, local application response, IP configuration, routes, DNS, firewall rules, cloud security groups, and application logs.

### 45. How would you automate daily server checks?

Write a script to gather selected metrics and statuses, save a timestamped report, validate errors, and schedule it with cron or an appropriate scheduler. Add monitoring and alerts if needed.

---

# 20. Quick Revision Cheat Sheet

| Requirement | Command |
|---|---|
| OS version | `cat /etc/os-release` |
| Kernel version | `uname -r` |
| System uptime | `uptime` |
| CPU details | `lscpu` |
| Memory | `free -h` |
| Live processes | `top` |
| Process list | `ps aux` |
| Find process by name | `pgrep -a NAME` |
| Disk space | `df -h` |
| Inode usage | `df -i` |
| Directory usage | `du -sh DIRECTORY` |
| Disk layout | `lsblk -f` |
| Mounted filesystems | `findmnt` |
| Large files | `find DIRECTORY -type f -size +100M` |
| Open files | `lsof` |
| Kernel messages | `dmesg` |
| System journal | `journalctl -b` |
| Service status | `systemctl status SERVICE_NAME` |
| Failed units | `systemctl --failed` |
| IP addresses | `ip addr` |
| Routes | `ip route` |
| Listening ports | `ss -tuln` |
| Test HTTP | `curl -I URL` |
| DNS lookup | `dig DOMAIN` |
| Search logs | `grep -in PATTERN FILE` |
| Process text by fields | `awk` |
| Transform text | `sed` |
| Compare files | `diff -u FILE1 FILE2` |
| Archive files | `tar -czf backup.tar.gz DIRECTORY` |
| Copy files remotely | `scp FILE USER@HOST:PATH` |
| Synchronize files | `rsync -av SOURCE/ DESTINATION/` |
| Check script syntax | `bash -n script.sh` |
| Inspect command help | `command --help` |
| Read command manual | `man command` |

---

# 21. Learning Checklist

## System information
- [ ] I can identify the Linux distribution and kernel version.
- [ ] I can inspect CPU and memory information.
- [ ] I can check uptime and system load.

## Processes and performance
- [ ] I can find a process by name.
- [ ] I can inspect CPU and memory usage.
- [ ] I understand termination signals.
- [ ] I can investigate performance using `vmstat` and `iostat`.

## Disk and filesystems
- [ ] I understand `df` versus `du`.
- [ ] I can inspect disks and mounts.
- [ ] I can check inode usage.
- [ ] I can find large files and directories.
- [ ] I understand how deleted open files can consume disk space.

## Logs and services
- [ ] I can use `journalctl`.
- [ ] I can inspect kernel messages.
- [ ] I can check service status.
- [ ] I can find failed systemd units.
- [ ] I can investigate application errors.

## Networking
- [ ] I can inspect IP addresses and routes.
- [ ] I can check listening ports.
- [ ] I can test DNS resolution.
- [ ] I can test HTTP endpoints.
- [ ] I understand basic SSH, SCP, and rsync usage.

## Automation and DevOps
- [ ] I can search and process text.
- [ ] I can compare configuration files.
- [ ] I can build a timestamped diagnostic report.
- [ ] I can validate scripts and troubleshoot failures.
- [ ] I can explain the interview questions in my own words.

---

# 22. Official Learning Resources

- [GNU Coreutils Manual](https://www.gnu.org/software/coreutils/manual/coreutils.html) — documentation for many essential Linux command-line utilities.
- [GNU Findutils Manual](https://www.gnu.org/software/findutils/manual/html_mono/find.html) — detailed information about `find` and related utilities.
- [GNU Grep Manual](https://www.gnu.org/software/grep/manual/grep.html) — searching text and using regular expressions.
- [Ubuntu Documentation](https://help.ubuntu.com/) — Ubuntu administration and troubleshooting.
- [systemd Documentation](https://www.freedesktop.org/software/systemd/man/) — documentation for systemd and related tools.
- [OpenSSH Manual Pages](https://www.openssh.com/manual.html) — SSH and related utilities.

---

## Final Advice

Do not try to memorise every command at once.

For each command, learn three things:

1. **Purpose:** What problem does it solve?
2. **Usage:** What are its most useful options?
3. **Interpretation:** How do you understand its output?

Start with `ps`, `top`, `free`, `df`, `du`, `find`, `grep`, `journalctl`, `systemctl`, `ip`, and `ss`. These commands are especially valuable when troubleshooting real Linux servers.

Then complete the **Linux Server Diagnostic Report** mini project and practise explaining your troubleshooting steps as if you were answering an interview question.

