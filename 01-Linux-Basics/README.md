# 🐧 01 — Linux Basics: Complete Beginner-to-DevOps Foundation

> **Goal:** Understand Linux from zero, confidently use an EC2 terminal, recognize how Linux works, and build a strong base for administration and DevOps.
>
> **How to use:** Read one section, run the practice commands on your own test EC2 instance, note the output, and tick the checklist. Do not run experiments on a production server.

## 📌 Table of Contents

1. [What is Linux?](#1-what-is-linux)
2. [Linux history, kernel, GNU, and distributions](#2-linux-history-kernel-gnu-and-distributions)
3. [Linux vs Windows; Ubuntu vs RHEL vs Amazon Linux](#3-linux-vs-windows-ubuntu-vs-rhel-vs-amazon-linux)
4. [Linux architecture and boot overview](#4-linux-architecture-and-boot-overview)
5. [Terminal, shell, Bash, and command syntax](#5-terminal-shell-bash-and-command-syntax)
6. [Setting up an AWS EC2 practice environment](#6-setting-up-an-aws-ec2-practice-environment)
7. [First essential commands and hands-on lab](#7-first-essential-commands-and-hands-on-lab)
8. [Paths, files, directories, and hidden files](#8-paths-files-directories-and-hidden-files)
9. [Help, documentation, and command discovery](#9-help-documentation-and-command-discovery)
10. [Standard input, output, errors, and exit codes](#10-standard-input-output-errors-and-exit-codes)
11. [Quoting, wildcards, variables, and command history](#11-quoting-wildcards-variables-and-command-history)
12. [Users, root, sudo, and basic security](#12-users-root-sudo-and-basic-security)
13. [Basic system information and monitoring](#13-basic-system-information-and-monitoring)
14. [Processes, services, packages, and networking overview](#14-processes-services-packages-and-networking-overview)
15. [Linux filesystem hierarchy overview](#15-linux-filesystem-hierarchy-overview)
16. [SSH, permissions, and EC2 safety](#16-ssh-permissions-and-ec2-safety)
17. [Practice labs and troubleshooting](#17-practice-labs-and-troubleshooting)
18. [Interview questions with answers](#18-interview-questions-with-answers)
19. [Revision cheatsheet and learning checklist](#19-revision-cheatsheet-and-learning-checklist)
20. [Next modules](#20-next-modules)

---

## 1. What is Linux?

Linux is an **open-source, Unix-like operating-system kernel**. In everyday usage, “Linux” also means a complete operating system built around that kernel, such as Ubuntu or Red Hat Enterprise Linux (RHEL).

An **operating system (OS)** manages computer hardware and provides services to programs and users. Linux is widely used on servers, cloud virtual machines, containers, networking equipment, and developer systems.

**Why DevOps engineers learn Linux:** Most automation tools, cloud servers, containers, build agents, and deployment environments use Linux or Linux-like command-line workflows.

**Analogy:** Hardware is a vehicle; the kernel is the engine/control system; user-space utilities and applications are the controls and features that make the vehicle usable.

**Key terms**

| Term | Meaning |
|---|---|
| Kernel | Core that manages CPU, memory, devices, and processes |
| Distribution (distro) | Complete OS built with the Linux kernel and other software |
| Shell | Program that interprets commands, e.g., Bash |
| Terminal | Interface used to interact with a shell |
| CLI | Command-line interface |
| GUI | Graphical user interface |
| Package manager | Tool to install, update, and remove software |
| Open source | Software whose source is available under a license permitting specified uses and modifications |

### Practice

```bash
uname -s
uname -r
cat /etc/os-release
```

Expected: `Linux`, a kernel release string, and OS identification such as `Ubuntu` or `Amazon Linux`.

---

## 2. Linux history, kernel, GNU, and distributions

- **Unix** inspired many operating-system ideas and interfaces.
- **Linux kernel** development began in 1991, initiated by Linus Torvalds.
- **GNU** provides many important user-space tools on common Linux distributions.
- A **distribution** packages the kernel with utilities, libraries, an init system, and a package manager.
- The kernel is not the same thing as the shell or the distribution.

Examples: Ubuntu, Debian, RHEL, Fedora, Rocky Linux, AlmaLinux, Amazon Linux, SUSE, and Alpine Linux. Their commands overlap, but package managers, defaults, support policies, and security systems may differ.

### Practice

```bash
cat /etc/os-release
uname -a
getconf LONG_BIT
```

`getconf LONG_BIT` commonly reports `64` on modern EC2 Linux images.

---

## 3. Linux vs Windows; Ubuntu vs RHEL vs Amazon Linux

| Feature | Ubuntu | RHEL | Amazon Linux |
|---|---|---|---|
| Family | Debian-based | Red Hat family | AWS-maintained Linux |
| Common package tool | `apt` | `dnf` (older RHEL: `yum`) | AL2023: `dnf` |
| Package format | `.deb` | `.rpm` | `.rpm` |
| Common SSH username on EC2 | `ubuntu` | `ec2-user` | `ec2-user` |
| Typical mandatory-access-control framework | AppArmor | SELinux | Varies by version/configuration |
| Primary use | Servers, cloud, desktops | Enterprise servers | AWS workloads |

**Linux vs Windows:** Linux commonly uses Bash and `/` paths, while Windows commonly uses PowerShell and drive-letter paths. Both support graphical and command-line administration. Linux filesystems are usually case-sensitive.

### Practice

```bash
cat /etc/os-release
command -v apt || true
command -v dnf || true
command -v bash
```

**Note:** Only use the package manager actually available on your instance. RHEL versions and images may differ; never assume all Linux systems are configured identically.

---

## 4. Linux architecture and boot overview

```text
+-------------------------------------+
| Applications: web server, Python... |
+-------------------------------------+
| Shell, utilities, libraries         |
+-------------------------------------+
| System services / init (systemd)    |
+-------------------------------------+
| Linux kernel                        |
| CPU scheduling, memory, I/O, files  |
+-------------------------------------+
| Hardware / EC2 virtual hardware     |
+-------------------------------------+
```

**Boot overview:** firmware/virtual firmware → bootloader → Linux kernel → init system (often systemd) → services → login. Details vary by image.

**Kernel responsibilities:** process scheduling, memory management, device drivers, filesystems, networking, and system calls.

**System calls:** controlled interface through which programs request kernel services.

### Practice

```bash
uname -r
ps -p 1 -o pid,comm,args
systemctl --version  # works when systemd is installed
```

On many EC2 distributions, PID 1 is `systemd`.

---

## 5. Terminal, shell, Bash, and command syntax

- **Terminal:** the window or remote session where you type commands.
- **Shell:** interprets and runs commands. Bash is common.
- **Prompt:** often ends with `$` for a regular user or `#` for root. Do not type the prompt itself.

General form:

```text
command [OPTIONS] [ARGUMENTS]
```

Example:

```bash
ls -l /home
```

Here `ls` is the command, `-l` is an option, and `/home` is an argument.

| Syntax | Meaning |
|---|---|
| `-l` | Short option |
| `--help` | Long option |
| `--` | End of option parsing for many commands |
| `;` | Run commands sequentially |
| `&&` | Run next command only if previous succeeded |
| `||` | Run next command only if previous failed |
| `#` | Begins a comment in typical shell contexts |

### Practice

```bash
echo "Hello, Linux!"
printf 'Welcome to DevOps\n'
echo "$SHELL"
bash --version
whoami; pwd
pwd && echo "pwd succeeded"
```

**Keyboard shortcuts:** `Tab` completion, `Ctrl+C` interrupt, `Ctrl+L` clear screen, `Ctrl+A` start of line, `Ctrl+E` end, `Ctrl+R` search history, `Ctrl+D` EOF/logout at empty prompt. `Ctrl+Z` suspends a foreground job; it does not terminate it.

---

## 6. Setting up an AWS EC2 practice environment

Use a **non-production** Ubuntu or Amazon Linux EC2 instance.

1. In AWS, select a suitable AMI, region, and instance type. Check free-tier eligibility and pricing for your account.
2. Create/select an SSH key pair, or use EC2 Instance Connect when supported.
3. Configure the security group. Prefer SSH access only from **your IP**, not from the whole internet. Use Session Manager where available.
4. Launch the instance; wait for status checks.
5. Connect with **EC2 Instance Connect** in the console, or via SSH from your computer.

Example SSH commands (replace placeholders; never paste actual private key material into notes):

```bash
# Ubuntu AMI
ssh -i /path/to/my-key.pem ubuntu@EC2_PUBLIC_IP

# Amazon Linux AMI
ssh -i /path/to/my-key.pem ec2-user@EC2_PUBLIC_IP
```

On Linux/macOS, private key permissions commonly need:

```bash
chmod 400 /path/to/my-key.pem
```

On Windows, use the appropriate SSH client and key-file permissions. Never upload `.pem` files, passwords, AWS credentials, or tokens to GitHub.

**After connecting:**

```bash
whoami
hostname
pwd
cat /etc/os-release
```

**Cost safety:** Stop or terminate lab resources when finished, and review AWS billing; some resources such as storage or public IPv4 addresses can still incur charges.

---

## 7. First essential commands and hands-on lab

| Command | What it does | Example |
|---|---|---|
| `pwd` | Print current working directory | `pwd` |
| `ls` | List entries | `ls -la` |
| `cd` | Change directory | `cd ~` |
| `mkdir` | Create directory | `mkdir -p lab/notes` |
| `rmdir` | Remove empty directory | `rmdir emptydir` |
| `touch` | Create empty file/update timestamps | `touch notes.txt` |
| `cat` | Display file contents | `cat notes.txt` |
| `echo` | Print text | `echo Hello` |
| `printf` | Format text | `printf 'Hi\n'` |
| `date` | Show date/time | `date` |
| `cal` | Show calendar (may not be installed) | `cal` |
| `clear` | Clear terminal screen | `clear` |
| `history` | Show shell history | `history` |
| `exit` | End current shell/session | `exit` |

### Guided lab: create your first project

```bash
cd ~
mkdir -p linux-learning/01-basics
cd linux-learning/01-basics
pwd
touch day01.txt
printf 'My first Linux lab\n' > day01.txt
cat day01.txt
ls -lah
```

Expected: `pwd` ends in `/linux-learning/01-basics`; `cat` prints `My first Linux lab`; `ls` shows `day01.txt`.

**Important:** `>` overwrites a file; `>>` appends. Do not redirect to important existing files.

---

## 8. Paths, files, directories, and hidden files

| Symbol | Meaning |
|---|---|
| `/` | Filesystem root |
| `~` | Your home directory (in a shell) |
| `.` | Current directory |
| `..` | Parent directory |
| `-` | Previous directory for `cd -` |
| `/home/ubuntu/file` | Absolute path |
| `notes/file.txt` | Relative path |

Linux uses `/` as the directory separator; names are usually case-sensitive (`File` differs from `file`). Filenames starting with `.` are hidden from normal `ls` output.

### Practice

```bash
cd ~
pwd
mkdir -p linux-learning/path-lab
cd linux-learning/path-lab
touch visible.txt .hidden.txt
ls
ls -la
cd ..
pwd
cd -
```

**Important:** In Linux, “everything is a file” is a useful mental model for many interfaces, but it is not literally true of every resource.

---

## 9. Help, documentation, and command discovery

| Command | Purpose |
|---|---|
| `command --help` | Quick usage (when supported) |
| `man command` | Manual page |
| `info command` | GNU info documentation (if installed) |
| `type command` | Shell classification: builtin, alias, executable |
| `command -v name` | Locate command as shell resolves it |
| `whatis command` | Short description (if database available) |
| `apropos keyword` | Search manual descriptions (if available) |

### Practice

```bash
mkdir --help
man mkdir
type cd
type ls
command -v bash
help cd
```

Press `q` to exit `man`. `cd` is usually a shell builtin, so `help cd` is useful in Bash. Some minimal EC2 images do not include `man` by default.

---

## 10. Standard input, output, errors, and exit codes

| Stream | Number | Meaning |
|---|---|---|
| stdin | `0` | Standard input |
| stdout | `1` | Normal output |
| stderr | `2` | Error output |

Operators:

- `>` write/overwrite stdout to a file
- `>>` append stdout to a file
- `2>` redirect stderr
- `|` pipe stdout to another command
- `2>&1` combine stderr with the current stdout destination

### Safe practice

```bash
cd ~/linux-learning/01-basics
echo 'first' > output.txt
echo 'second' >> output.txt
cat output.txt
ls /this-path-does-not-exist 2> errors.txt
cat errors.txt
printf 'alpha\nbeta\n' | wc -l
true
echo $?
false
echo $?
```

Expected: `wc -l` prints `2`; `true` returns exit code `0` (success), `false` returns `1` (failure). `$?` holds the status of the **immediately previous** command, so check it right away.

---

## 11. Quoting, wildcards, variables, and command history

**Quoting:**

- `'single quotes'` preserve most characters literally.
- `"double quotes"` allow variable and command substitution.
- `\` escapes the next character in many shell contexts.

**Globbing:** `*` matches many characters; `?` matches one; `[abc]` matches one from a set. Dotfiles generally need explicit patterns.

### Practice

```bash
cd ~/linux-learning/01-basics
name='Linux'
echo '$name'
echo "$name"
printf 'one\n' > one.txt
printf 'two\n' > two.txt
ls *.txt
echo "Today is $(date +%F)"
export MY_TOPIC=DevOps
echo "$MY_TOPIC"
history | tail -n 5
```

**Environment vs shell variables:** `export` makes a variable available to child processes. A variable assigned without `export` is generally only a shell variable.

**PATH:** the colon-separated list of directories searched for executables.

```bash
printf '%s\n' "$PATH"
```

**Safety:** Shell history may store typed commands. Do not type secrets into command lines or copy secrets into GitHub notes.

---

## 12. Users, root, sudo, and basic security

Linux is a multiuser system. Users have IDs (UIDs) and belong to groups (GIDs). The root account (UID 0) has powerful administrative privileges. `sudo` runs authorized commands with elevated privileges.

### Practice (read-only)

```bash
whoami
id
id -u
groups
getent passwd "$(whoami)"
```

`sudo -l` lists your permitted sudo commands (may ask for authentication):

```bash
sudo -l
```

**Avoid:** running unknown commands with `sudo`, routinely using root, or editing system files before you understand the impact. User creation, permissions, and sudoers configuration belong in `03-User-Management`.

---

## 13. Basic system information and monitoring

| Command | Purpose |
|---|---|
| `hostname` | Machine hostname |
| `hostnamectl` | Host and OS details (on systemd systems) |
| `uname -a` | Kernel/system information |
| `uptime` | Time since boot and load averages |
| `free -h` | Memory usage |
| `df -h` | Filesystem capacity |
| `du -sh DIR` | Space used by directory |
| `lscpu` | CPU details (if installed) |
| `nproc` | Number of processing units available |
| `date` | Date/time |
| `timedatectl` | Time configuration (on systemd systems) |

### Practice

```bash
hostname
uname -r
uptime
free -h
df -h
nproc
date
```

**Interpretation:** `free` reports memory; `df` reports filesystem space; `uptime` includes load averages. Load average is not a percentage of CPU usage.

---

## 14. Processes, services, packages, and networking overview

These are **introductions**; practice them deeply in later modules.

### Processes

```bash
ps -ef | head
top -b -n 1 | head  # top batch mode; may differ by OS
```

A **process** is a running program instance. A **PID** is its process ID.

### Services (systemd systems)

```bash
systemctl --version
systemctl list-units --type=service --state=running --no-pager | head
```

A **service** is commonly a background-managed workload. Avoid starting/stopping system services until you know their role.

### Packages

```bash
# Ubuntu/Debian: inspect only
apt --version

# RHEL/Amazon Linux 2023: inspect only
dnf --version
```

Package managers install software and resolve dependencies. Updating packages may restart services; do it intentionally.

### Networking

```bash
ip -br addr
ip route
hostname -I
```

`ip` may not be installed on every minimal image. IP addresses can be private or public; EC2 often uses private addresses inside a VPC. Detailed SSH, DNS, ports, firewall, and connectivity labs belong in the networking module.

---

## 15. Linux filesystem hierarchy overview

| Path | Common purpose |
|---|---|
| `/` | Root of filesystem tree |
| `/home` | User home directories |
| `/root` | Root user's home directory |
| `/etc` | System configuration |
| `/var` | Variable data such as logs |
| `/tmp` | Temporary files (retention varies) |
| `/usr` | Most user-space programs and shared resources |
| `/bin`, `/sbin` | Essential commands; often symlinks into `/usr` on modern systems |
| `/lib`, `/lib64` | Libraries; often merged/symlinked on modern systems |
| `/boot` | Boot-related files |
| `/dev` | Device interfaces |
| `/proc` | Virtual process/kernel information |
| `/sys` | Virtual device/kernel information |
| `/run` | Runtime state |
| `/opt` | Optional third-party software |
| `/mnt`, `/media` | Mount points |

### Practice (read-only)

```bash
ls /
ls -ld /home /etc /var /tmp /usr /proc /sys
cat /proc/version
```

**Do not delete or modify system directories during practice.** Learn the full hierarchy, mount points, file operations, and disk tools in `02-Linux-File-System`.

---

## 16. SSH, permissions, and EC2 safety

- **SSH** provides encrypted remote shell access.
- **Security groups** control allowed network traffic to EC2 instances.
- **IAM** controls AWS API permissions; it is different from Linux users and `sudo`.
- **Key pairs** authenticate SSH connections when configured.
- **Principle of least privilege:** grant only the access needed.
- **Public IP** can change after stopping/starting an instance unless a stable address mechanism is configured.

### Read-only checks

```bash
whoami
id
ls -ld ~
ss -tuln | head  # if ss is installed
```

**Never commit** private keys, `.env` files containing secrets, credentials, or sensitive work data. Keep AWS security group SSH access restricted to trusted sources.

---

## 17. Practice labs and troubleshooting

### Lab A — Identify your server

```bash
whoami
hostname
cat /etc/os-release
uname -r
uptime
free -h
df -h
```

**Task:** Write down your username, OS name/version, kernel version, uptime, memory, and disk capacity.

### Lab B — Navigate and create a workspace

```bash
cd ~
mkdir -pv linux-learning/lab-b/{notes,output,backup}
cd linux-learning/lab-b
pwd
ls -la
```

**Task:** Explain why `-p` and `-v` are useful.

### Lab C — Create and inspect text

```bash
printf 'Linux\nAWS\nDevOps\n' > notes/topics.txt
cat notes/topics.txt
wc -l notes/topics.txt
head -n 2 notes/topics.txt
tail -n 1 notes/topics.txt
```

Expected line count: `3`.

### Lab D — Find help without searching the web

```bash
mkdir --help
man ls
help cd
```

If `man` is unavailable, use `ls --help` and record that the manual package may need installation.

### Lab E — Error handling

```bash
cd ~/linux-learning/lab-b
ls missing-file 2> output/error.log
cat output/error.log
```

**Task:** Explain stdout vs stderr.

### Common errors

| Error | Likely cause | First check |
|---|---|---|
| `command not found` | Command missing or not in PATH | `command -v NAME`, `echo "$PATH"` |
| `Permission denied` | Insufficient access or non-executable file | `id`, `ls -l FILE` |
| `No such file or directory` | Wrong path or missing parent | `pwd`, `ls -la`, `ls PARENT` |
| `File exists` | Already created | `ls -ld NAME`; use `mkdir -p` when appropriate |
| `Connection timed out` (SSH) | Network/security group/routing issue | Check instance status, IP, inbound rules |
| `Permission denied (publickey)` (SSH) | Wrong user/key or key permissions | Check AMI default username and SSH key |
| `man: command not found` | Manual reader not installed | Use `--help` until installed |

### Lab completion notes (fill in)

```text
Date:
EC2 AMI / Linux distribution:
Commands practiced:
What I understood:
Error encountered:
How I fixed it:
What I will revise tomorrow:
```

---

## 18. Interview questions with answers

1. **What is Linux?** An open-source Unix-like kernel; the term often refers to operating systems built around it.
2. **What is a kernel?** Core OS component managing hardware resources and offering system calls.
3. **What is a distribution?** A packaged operating system combining Linux with tools, libraries, and software management.
4. **What is a shell?** A command interpreter; Bash is a common example.
5. **Terminal vs shell?** Terminal is the interface; shell executes commands.
6. **What is Bash?** Bourne Again SHell, a widely used Unix shell and scripting language.
7. **What is root?** The privileged Linux user, typically UID 0.
8. **What is sudo?** A way for authorized users to run commands with another user's privileges, commonly root.
9. **What is an absolute path?** A path starting at `/`.
10. **What is a relative path?** A path interpreted from the current directory.
11. **What does `pwd` do?** Prints the current working directory.
12. **What does `mkdir -p` do?** Creates missing parent directories and succeeds if the target directory already exists.
13. **What is a hidden file?** A filename beginning with `.`; `ls -a` shows it.
14. **What is `PATH`?** Environment variable listing executable search directories.
15. **What is a process?** An instance of a running program.
16. **What is PID 1?** First userspace process, often the init/service manager.
17. **What is an exit code of zero?** Conventionally success.
18. **Difference between `>` and `>>`?** Overwrite versus append.
19. **What is a pipe?** `|` connects one command's stdout to another command's stdin.
20. **What is SSH?** A protocol for secure remote connections.
21. **What is the difference between IAM and Linux users?** IAM authorizes AWS actions; Linux accounts control OS-level identity and permissions.
22. **Ubuntu vs RHEL package manager?** Typically `apt` vs `dnf`.
23. **What is `/etc`?** Common location for system configuration files.
24. **What is `/var`?** Variable data such as logs and caches.
25. **What does `uname -r` show?** Running kernel release.
26. **What is the difference between `df` and `du`?** Filesystem free/used space versus space consumed by paths.
27. **What is a system call?** A program's controlled request for a kernel service.
28. **Why is Linux common in DevOps?** Server/cloud prevalence, automation tooling, containers, and scriptable administration.
29. **What does `Ctrl+C` do?** Sends an interrupt signal to the foreground process group in a typical terminal.
30. **Why avoid using root routinely?** Reduces accidental or unauthorized system-wide changes.

---

## 19. Revision cheatsheet and learning checklist

### Quick command sheet

```bash
whoami                 # Current username
id                     # User and group IDs
pwd                    # Current directory
ls -lah                # Detailed list including hidden files
cd ~                   # Home directory
cd ..                  # Parent directory
mkdir -pv a/b/c        # Create nested directories with messages
touch file.txt         # Create/update file timestamp
cat file.txt           # Display file contents
uname -r               # Kernel release
cat /etc/os-release    # Distribution details
hostname               # Hostname
uptime                 # Uptime and load
free -h                # Memory
df -h                  # Filesystem space
history                # Shell history
command -v bash        # Resolve command
man ls                 # Manual (if installed)
```

### Learning checklist

- [ ] I can explain OS, Linux, kernel, distro, shell, and terminal.
- [ ] I can compare Ubuntu, RHEL, and Amazon Linux.
- [ ] I can describe the Linux architecture and boot sequence at a high level.
- [ ] I can connect safely to my EC2 lab instance.
- [ ] I can identify OS, kernel, user, hostname, CPU, memory, and disk.
- [ ] I understand command syntax, options, arguments, and quoting.
- [ ] I can use `pwd`, `ls`, `cd`, `mkdir`, `touch`, `cat`, and `echo`.
- [ ] I understand absolute/relative paths, `~`, `.`, and `..`.
- [ ] I can read `--help` and `man` pages.
- [ ] I understand stdin, stdout, stderr, pipes, and exit codes.
- [ ] I understand variables, PATH, wildcards, and history.
- [ ] I can explain root, sudo, IAM, SSH, and basic security.
- [ ] I recognize important top-level filesystem directories.
- [ ] I completed Labs A–E and recorded my own outputs.
- [ ] I answered the interview questions without reading the answers.

### Suggested 5-day practice plan

| Day | Topics | Hands-on |
|---|---|---|
| 1 | Sections 1–4 | OS/kernel/architecture identification |
| 2 | Sections 5–7 | EC2 terminal, shell, basic commands |
| 3 | Sections 8–11 | Paths, help, redirects, variables |
| 4 | Sections 12–16 | Users, system details, service/network overview |
| 5 | Sections 17–19 | Labs, troubleshooting, interview revision |

---

## 20. Next modules

Linux Basics establishes the foundation, **not every Linux administration topic**. Continue with:

- **`02-Linux-File-System/`** — file operations, filesystem hierarchy in depth, links, disk usage, mounts, partitions, inodes, filesystems.
- **`03-User-Management/`** — users, groups, ownership, chmod, ACLs, sudoers, permissions, security.
- **`04-Shell-Scripting/`** — Bash scripting, conditions, loops, functions, automation, text processing.
- **Linux system administration and networking** — processes, systemd, logs, packages, cron, SSH, TCP/IP, DNS, firewalls, performance and troubleshooting. Consider dedicated folders later to keep these topics organized.
- **`05-Git-GitHub/`** through **`10-DevOps-Projects/`** — DevOps tooling and practical projects.

> **Revision rule:** Read → Run → Understand → Write the output → Repeat without looking.
