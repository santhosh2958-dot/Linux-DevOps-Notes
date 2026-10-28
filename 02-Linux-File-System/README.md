# 02 — Linux File System and File Management Commands

> Beginner-friendly reference for Ubuntu, RHEL, and Amazon Linux. Practice commands inside a dedicated lab directory on your EC2 instance. Commands that delete files, change permissions, mount disks, or modify system paths are clearly marked—understand them before running.

## Table of contents

1. [Learning goals](#1-learning-goals)
2. [Safety and lab setup](#2-safety-and-lab-setup)
3. [Linux filesystem hierarchy](#3-linux-filesystem-hierarchy)
4. [Paths and navigation](#4-paths-and-navigation)
5. [List and inspect files](#5-list-and-inspect-files)
6. [Create files and directories](#6-create-files-and-directories)
7. [Read file contents](#7-read-file-contents)
8. [Copy, move, rename, and remove](#8-copy-move-rename-and-remove)
9. [Search for files and content](#9-search-for-files-and-content)
10. [File metadata and disk usage](#10-file-metadata-and-disk-usage)
11. [Links: hard and symbolic](#11-links-hard-and-symbolic)
12. [Archives and compression](#12-archives-and-compression)
13. [Permissions and ownership overview](#13-permissions-and-ownership-overview)
14. [Redirection and pipes](#14-redirection-and-pipes)
15. [Mounts and filesystems](#15-mounts-and-filesystems)
16. [Useful wildcards and expansions](#16-useful-wildcards-and-expansions)
17. [Ubuntu vs RHEL/Amazon Linux](#17-ubuntu-vs-rhelamazon-linux)
18. [Common errors and fixes](#18-common-errors-and-fixes)
19. [EC2 practice labs](#19-ec2-practice-labs)
20. [Interview questions](#20-interview-questions)
21. [Quick revision cheat sheet](#21-quick-revision-cheat-sheet)
22. [Completion checklist](#22-completion-checklist)

---

## 1. Learning goals

After practicing this module, you should be able to:

- Navigate Linux directories using `pwd`, `cd`, and `ls`.
- Understand absolute paths, relative paths, hidden files, and the filesystem hierarchy.
- Create, inspect, read, copy, move, rename, and remove files/directories.
- Search for files and text using `find`, `grep`, and related tools.
- Check file metadata, file sizes, free disk space, and directory usage.
- Explain symbolic links and hard links.
- Create and extract archives using `tar` and compression tools.
- Understand basic permissions, mounts, and filesystems.
- Practice safely on an EC2 instance without modifying important system files.

## 2. Safety and lab setup

### 2.1 Identify the operating system

```bash
cat /etc/os-release
whoami
hostname
pwd
```

`/etc/os-release` identifies the distribution. `whoami` prints the current user. `hostname` prints the machine name. `pwd` prints the current working directory.

### 2.2 Create a dedicated practice area

Run these commands as your normal EC2 user:

```bash
cd ~
mkdir -p linux-practice/02-file-system
cd ~/linux-practice/02-file-system
pwd
```

Expected path is commonly `/home/ubuntu/linux-practice/02-file-system` on Ubuntu, or `/home/ec2-user/linux-practice/02-file-system` on Amazon Linux. Your actual username may differ.

Create sample content:

```bash
mkdir -p projects/{app,logs,backup}
touch notes.txt
printf 'Linux file system practice\nmkdir creates directories\nfind searches for files\n' > notes.txt
mkdir -p sample/subdir
printf 'Hello from Linux\n' > sample/hello.txt
```

Check the layout:

```bash
find . -maxdepth 4 -print
```

### Safety rules

- Practice inside `~/linux-practice`; do not experiment in `/`, `/etc`, `/usr`, `/var`, or other system directories.
- `rm` permanently removes files in normal command-line use. Check the path before running it.
- Never practice `rm -rf` with an unverified path or elevated privileges.
- Do not run destructive disk commands (`mkfs`, `fdisk`, `parted`, `wipefs`) on an EC2 root disk.
- Do not use `sudo` unless a task specifically requires it and you understand why.
- A private GitHub repository is not a safe place for passwords, private keys, AWS credentials, or confidential company data.

---

## 3. Linux filesystem hierarchy

Linux organizes files under a single root directory: `/`. Directories and mounted disks appear within this tree.

| Path | Typical purpose | What to remember |
|---|---|---|
| `/` | Root of the entire directory tree | Every absolute path starts here |
| `/home` | Regular users' home directories | Your personal working files |
| `/root` | Root user's home directory | Not the same as `/` |
| `/etc` | System and service configuration | Avoid casual edits |
| `/var` | Variable data such as logs, caches, queues | Logs commonly live in `/var/log` |
| `/tmp` | Temporary files | May be cleaned automatically |
| `/usr` | Most user-space programs, libraries, documentation | Not the same as `/home` |
| `/usr/bin` | Common user commands | e.g. many installed utilities |
| `/usr/sbin` | System administration commands (layout varies) | Some commands are elsewhere on modern systems |
| `/bin`, `/sbin` | Traditional command locations | Often links to `/usr/bin` or `/usr/sbin` on modern distributions |
| `/opt` | Optional third-party application software | Vendor applications may use it |
| `/dev` | Device files | Represents devices and virtual devices |
| `/proc` | Virtual process/kernel information | Generated by the kernel |
| `/sys` | Virtual device/kernel interface | Used by kernel and device tools |
| `/run` | Runtime state since boot | Usually temporary |
| `/boot` | Bootloader and kernel-related files | Do not modify casually |
| `/mnt` | Common temporary mount point | Often used by administrators |
| `/media` | Removable-media mount points on some systems | Desktop-oriented convention |
| `/srv` | Data served by system services | Use depends on service/admin design |

Inspect a few locations:

```bash
ls /
ls /home
ls -ld /etc /var /tmp
```

**Important:** Do not delete or rename system directories to “practice” file commands.

## 4. Paths and navigation

### `pwd` — print working directory

```bash
pwd
```

Shows the directory your shell is currently in.

### `cd` — change directory

```bash
cd /etc                 # absolute path
cd ..                   # parent directory
cd .                    # current directory (no movement)
cd ~                    # your home directory
cd                       # also usually returns home
cd -                    # previous working directory
cd ~/linux-practice     # path relative to your home
```

`cd` is a shell built-in, not usually a separate executable. You cannot use it to change the parent shell's directory from a child process.

### Absolute vs relative paths

- Absolute: `/home/ubuntu/linux-practice/notes.txt` — begins at `/`.
- Relative: `sample/hello.txt` — interpreted from your current directory.
- `.` means current directory; `..` means parent directory; `~` represents your home directory in the shell.

### `realpath` and `readlink`

```bash
realpath notes.txt
readlink -f sample/hello.txt
```

`realpath` prints a canonical path. `readlink -f` resolves a path (including symbolic links) when supported and resolvable. If unavailable, the package/tool version may differ.

### `basename` and `dirname`

```bash
basename /home/ubuntu/report.txt    # report.txt
dirname /home/ubuntu/report.txt     # /home/ubuntu
```

These extract the final path component and its parent path, respectively.

## 5. List and inspect files

### `ls` — list directory contents

```bash
ls
ls sample
ls -l                   # long format: permissions, links, owner, group, size, date
ls -a                   # include hidden entries beginning with .
ls -la                  # long format including hidden entries
ls -lh                  # human-readable file sizes
ls -lt                  # newest modification times first
ls -ltr                 # oldest modification times first
ls -R                   # recursive listing; can produce lots of output
ls -ld sample           # information about directory itself, not its contents
```

Useful options:
- `-a`: all entries, including hidden files.
- `-l`: long listing.
- `-h`: human-readable sizes, generally paired with `-l`.
- `-t`: sort by modification time.
- `-r`: reverse sort order.
- `-R`: recursively list subdirectories.
- `-d`: list directory entry itself.

### Hidden files

Names beginning with `.` are hidden from ordinary `ls` output:

```bash
touch .env-example
ls
ls -a
```

“Hidden” is a display convention, not a security feature. Never store secrets in a file merely because its name begins with a dot.

### `tree` (optional)

```bash
tree .
```

If not installed, use `find . -maxdepth 3 -print`. Installation command depends on distribution:
- Ubuntu: `sudo apt update && sudo apt install tree`
- RHEL/Amazon Linux: package availability and manager vary; use the configured package manager.

## 6. Create files and directories

### `mkdir` — make directories

```bash
mkdir reports
mkdir app logs backup
mkdir -p project/src/main
mkdir -v -p demo/a/b
mkdir -m 700 private
```

- `-p`: create missing parent directories; don't error if they already exist.
- `-v`: print each created directory.
- `-m MODE`: set permissions at creation (e.g. `700` gives owner full access and no access to group/others, subject to filesystem and security rules).

Check:

```bash
ls -ld reports private
```

SELinux options such as `-Z` and `--context` are relevant on compatible SELinux systems and may not work on Ubuntu or every filesystem. GNU/Linux implementations can differ; check `mkdir --help` on your instance.

### `touch` — create empty files or update timestamps

```bash
touch empty.txt
touch file1.txt file2.txt
touch notes.txt
```

If a file does not exist, `touch` creates an empty file. If it exists, it normally updates access/modification timestamps without erasing content.

### `printf` — write known sample content

```bash
printf 'first line\nsecond line\n' > example.txt
```

`>` creates or overwrites the destination. Use caution because it replaces existing file content.

### `install` — copy a file and optionally set mode/ownership

```bash
install -D -m 644 notes.txt output/notes.txt
```

On common GNU systems, `-D` creates required parent directories. Check `install --help` if the option differs on your system.

### `mktemp` — create temporary files/directories safely

```bash
mktemp
mktemp -d
```

It prints the generated path. Save the path if you need to use or clean up the temporary item later.

## 7. Read file contents

Use the sample `notes.txt` created in the lab.

### `cat`

```bash
cat notes.txt
cat file1.txt file2.txt
```

Prints whole file(s). Avoid dumping very large files directly to a terminal.

### `less` and `more`

```bash
less notes.txt
more notes.txt
```

`less` lets you scroll/search through output. Press `q` to exit. `more` is a simpler pager.

### `head` and `tail`

```bash
head notes.txt
head -n 2 notes.txt
tail notes.txt
tail -n 5 notes.txt
tail -f application.log
```

- `head`: beginning of file; default is 10 lines.
- `tail`: end of file; default is 10 lines.
- `tail -f`: follow appended content, useful for logs; press Ctrl+C to stop.

### `nl` and `tac`

```bash
nl -ba notes.txt
tac notes.txt
```

`nl -ba` numbers all lines (including blank lines). `tac` prints lines in reverse order.

### `file`

```bash
file notes.txt
file /bin/ls
```

Identifies the type of content by inspecting the file; it does not simply rely on the filename extension.

## 8. Copy, move, rename, and remove

**Practice only on files under `~/linux-practice/02-file-system`.**

### `cp` — copy files

```bash
cp notes.txt notes-copy.txt
cp notes.txt backup-notes.txt
cp notes.txt projects/
cp -i notes.txt notes-copy.txt
cp -v notes.txt another-copy.txt
```

- `-i`: ask before overwriting.
- `-v`: show actions.
- `-n`: do not overwrite an existing destination on implementations that support it.
- `-p`: preserve common metadata such as timestamps and mode where possible.

Copy directories recursively:

```bash
cp -r sample sample-copy
cp -a sample sample-archive-copy
```

`-r` recursively copies directories. `-a` is archive mode on GNU `cp`, preserving symlinks and metadata as far as possible. Check your platform's help for details.

### `mv` — move or rename

```bash
mv notes-copy.txt renamed-notes.txt
mv renamed-notes.txt backup/
mv -i notes.txt backup/notes.txt
mv -v backup-notes.txt moved-notes.txt
```

Moving within the same filesystem is often a rename operation. Across filesystems it may require copying and removing the original. `-i` prompts before overwrite.

### `rm` — remove files (DANGEROUS)

```bash
rm -i moved-notes.txt
rm -v another-copy.txt
```

- `-i`: prompt before each removal.
- `-v`: report removed files.
- `-r`: recursively remove directories and their contents.
- `-f`: force; suppress many prompts/errors.

To remove a practice directory, first inspect it:

```bash
find sample-copy -maxdepth 3 -print
```

Then, only if you are certain it is the correct lab directory:

```bash
rm -rI sample-copy
```

`-I` asks for confirmation for a recursive removal in GNU `rm` under certain conditions. Option support differs. **Never run `rm -rf` with a guessed or unverified path. Never test deletion against system directories or your EC2 root filesystem.**

### `rmdir` — remove empty directories only

```bash
mkdir empty-lab
rmdir empty-lab
```

It fails if the directory is not empty. This makes it safer than recursive deletion for empty directories.

### `shred` (special-purpose, not a guaranteed secure erase)

`shred` attempts to overwrite file contents on supported storage, but it is not reliable for secure erasure on SSDs, copy-on-write filesystems, snapshots, or cloud storage. Do not use it as a security guarantee.

## 9. Search for files and content

### `find` — search by path, name, type, size, or time

Run from your lab:

```bash
find .
find . -type f
find . -type d
find . -name '*.txt'
find . -iname '*.TXT'
find . -type f -name '*.log'
find . -maxdepth 2 -type f
find . -type f -size +1M
find . -type f -mmin -60
find . -empty
```

Meaning:
- `-type f`: regular files; `-type d`: directories.
- `-name`: case-sensitive pattern; `-iname`: case-insensitive.
- `-maxdepth`: limit traversal depth.
- `-size +1M`: larger than approximately 1 MiB according to `find`'s size units.
- `-mmin -60`: modified within the last 60 minutes.
- `-empty`: empty files or directories.

Safe action example:

```bash
find . -type f -name '*.tmp' -print
```

Preview first; don't combine a broad search with deletion unless you fully understand the match list.

### `grep` — search inside text

```bash
grep 'Linux' notes.txt
grep -i 'linux' notes.txt
grep -n 'directory' notes.txt
grep -in 'linux' notes.txt
grep -r 'pattern' sample/
grep -R --include='*.txt' 'Linux' .
```

- `-i`: ignore case.
- `-n`: show line numbers.
- `-r` / `-R`: recursive search; exact symlink handling differs.
- `--include`: limit matching filenames in GNU grep.

### `locate` (optional index-based search)

```bash
locate notes.txt
```

`locate` searches a database, which may be missing or outdated. Updating the database may require an installed service and elevated privileges; don't assume it is available on EC2.

### `which`, `command -v`, and `whereis`

```bash
command -v ls
which ls
whereis ls
```

`command -v` is a useful shell-portable way to ask how a command is resolved. `which` and `whereis` availability/behavior can vary.

## 10. File metadata and disk usage

### `stat` — detailed metadata

```bash
stat notes.txt
stat sample
```

Shows information such as size, permissions, owner, inode, and timestamps. Directory size shown by `stat` is not the total size of everything inside it.

### `du` — disk space used by files/directories

```bash
du -sh .
du -sh sample
du -h --max-depth=1 .
du -ah . | sort -h
```

- `-s`: summary.
- `-h`: human-readable units.
- `-a`: include files as well as directories.
- `--max-depth=1`: one directory level on GNU `du`.

### `df` — filesystem free/used space

```bash
df -h
df -h .
df -T
```

`df` reports space for mounted filesystems. `df -T` displays filesystem type on GNU/Linux.

### `lsblk` — block devices

```bash
lsblk
lsblk -f
```

Displays block devices, partitions, mount points, and filesystem information. It is an inspection tool; it does not format disks.

### `du` vs `df`

- `du`: estimates space used by files/directories.
- `df`: reports allocated/available space on a mounted filesystem.

They can disagree because of deleted-but-open files, reserved blocks, mount points, filesystem metadata, and other reasons.

### `file`, `stat`, `du`, and `df` at a glance

| Command | Answers |
|---|---|
| `file path` | What type of file is this? |
| `stat path` | What are its metadata and timestamps? |
| `du -sh path` | How much space does this path use? |
| `df -h path` | How much space is free on the filesystem containing it? |
| `lsblk -f` | Which block devices/filesystems are present? |

## 11. Links: hard and symbolic

### Symbolic (soft) link

```bash
printf 'Original content\n' > original.txt
ln -s original.txt original-link.txt
ls -l original-link.txt
cat original-link.txt
```

A symbolic link stores a path to another file/directory. It can cross filesystem boundaries and can become dangling if the target is moved or removed.

### Hard link

```bash
ln original.txt original-hardlink.txt
ls -li original.txt original-hardlink.txt
```

Hard links point to the same underlying inode/data. Editing either name changes the same file content. They generally cannot cross filesystem boundaries, and hard links to directories are normally prohibited for ordinary users.

### Compare

| Feature | Symbolic link (`ln -s`) | Hard link (`ln`) |
|---|---|---|
| Refers to | A path | Same inode |
| Can cross filesystems | Yes | No |
| Can link directories | Yes, where supported | Normally no for users |
| Target removed | Link may dangle | Data remains while another hard link exists |

Try:

```bash
readlink original-link.txt
stat original.txt original-hardlink.txt
```

## 12. Archives and compression

### `tar` — archive files/directories

Create an archive:

```bash
tar -cvf sample.tar sample/
```

List contents:

```bash
tar -tvf sample.tar
```

Extract into the current directory:

```bash
mkdir extracted
tar -xvf sample.tar -C extracted
```

### gzip-compressed tar archive

```bash
tar -czvf sample.tar.gz sample/
tar -tzvf sample.tar.gz
mkdir extracted-gzip
tar -xzvf sample.tar.gz -C extracted-gzip
```

Flags:
- `-c`: create archive.
- `-x`: extract archive.
- `-t`: list archive contents.
- `-v`: verbose output.
- `-f`: archive filename follows.
- `-z`: use gzip compression/decompression.

### Other tools

```bash
gzip notes.txt
gunzip notes.txt.gz
zip -r sample.zip sample/
unzip -l sample.zip
unzip sample.zip -d extracted-zip
```

`zip` and `unzip` may need installation. Compressing with `gzip` normally replaces the original with a `.gz` file; use copies if you need to retain the original.

**Safety:** Inspect archive contents before extracting files obtained from untrusted sources. Avoid blindly extracting archives as root.

## 13. Permissions and ownership overview

Detailed user and permission management belongs in `03-User-Management`, but file management requires the basics.

View permissions:

```bash
ls -l notes.txt
```

Typical output format:

```text
-rw-r--r-- 1 ubuntu ubuntu 30 Oct 28 09:00 notes.txt
```

The first character indicates file type (`-` regular file, `d` directory, `l` symbolic link). The next nine characters represent owner, group, and others' permissions.

### `chmod` — change mode

```bash
chmod 600 notes.txt
chmod u+x script.sh
chmod g-w notes.txt
```

- `600`: owner can read/write; group and others have no permissions.
- `u+x`: add execute permission for owner.
- `g-w`: remove write permission from group.

### `chown` — change ownership

```bash
ls -l notes.txt
```

Ownership changes often require elevated privileges. Learn with a disposable test file in a controlled lab before running `sudo chown`. Do not change ownership of system files casually.

### Directory execute permission

On a directory, `x` means traverse/search. Read permission allows listing names; write permission allows creating/removing entries when combined with the needed permissions. Removing directory execute permission can prevent access even if a file itself is readable.

## 14. Redirection and pipes

### Standard output redirection

```bash
printf 'first\n' > output.txt
printf 'second\n' >> output.txt
cat output.txt
```

- `>` creates or overwrites.
- `>>` appends.

### Standard error

```bash
ls missing-file 2> error.txt
cat error.txt
```

`2>` redirects standard error (file descriptor 2). `2>>` appends errors.

### Combine output and errors

```bash
some-command > output.log 2>&1
```

This sends standard output and standard error to the same destination in common shells. Order matters: `> output.log 2>&1` is not equivalent to `2>&1 > output.log`.

### Pipes

```bash
ls -l | less
find . -type f | sort
du -ah . | sort -h | tail
```

A pipe `|` sends one command's standard output to another command's standard input.

### `tee`

```bash
printf 'Practice complete\n' | tee status.txt
```

`tee` writes to both the terminal and a file. Use `tee -a` to append.

## 15. Mounts and filesystems

These commands are mainly for inspection in this learning module. Do not mount, format, or partition your EC2 root disk as an exercise.

```bash
findmnt
findmnt /
mount | head
df -hT
lsblk -f
```

- `findmnt`: shows mounted filesystems.
- `mount`: displays or manages mounts; changing mounts may require root.
- `df -hT`: free/used capacity and filesystem type.
- `lsblk -f`: block devices, filesystem labels/UUIDs, and mount points.

### Important concepts

- **Filesystem:** organizes data on a storage device (e.g. ext4, XFS).
- **Partition:** a logical division of a disk.
- **Mount point:** directory where a filesystem is attached to the directory tree.
- **UUID:** persistent identifier often used in mount configuration.
- **Inode:** filesystem metadata record associated with a file, including its type, permissions, owner, timestamps, and data-block references (exact details vary).

### `mount` and `umount`

These can change system state and usually require elevated privileges. Learn their syntax from help/manual pages and practice only with a separate, disposable lab volume under instructor guidance. Never unmount a volume that is in use or format an existing data volume.

## 16. Useful wildcards and expansions

Use a lab directory and preview matches before applying a command that changes or deletes files.

| Pattern | Meaning |
|---|---|
| `*.txt` | Names ending in `.txt` |
| `file?.txt` | `?` matches one character |
| `file[123].txt` | One character from 1, 2, or 3 |
| `*` | Zero or more characters in shell filename expansion |
| `{a,b,c}` | Bash brace expansion into multiple strings |

Examples:

```bash
touch a.txt b.txt c.log
ls *.txt
printf '%s\n' file{1,2,3}.txt
mkdir -p practice/{src,docs,logs}
```

Quotes matter:
- `ls *.txt` lets the shell expand matching names.
- `echo '*.txt'` keeps the asterisk literal.
- `find . -name '*.txt'` quotes the pattern so the shell passes it to `find`.

## 17. Ubuntu vs RHEL/Amazon Linux

Most core file commands (`pwd`, `cd`, `ls`, `mkdir`, `cp`, `mv`, `rm`, `find`, `grep`, `tar`) are available across common Linux distributions. Small option differences can occur depending on implementation and version.

| Task | Ubuntu | RHEL / Amazon Linux |
|---|---|---|
| User home | Often `/home/ubuntu` on EC2 Ubuntu | Often `/home/ec2-user` |
| Package manager | `apt` | `dnf` or `yum`, depending on release |
| Default filesystem | Depends on image/storage setup | Depends on image/storage setup |
| SELinux | Usually not enabled by default; AppArmor commonly used | SELinux commonly available, depending on configuration |
| Core commands | GNU utilities on typical server images | GNU utilities on typical server images |

Check your actual system instead of assuming:

```bash
cat /etc/os-release
command -v mkdir
mkdir --help
find --version
```

The `mkdir -Z` and `--context` options are SELinux-specific and not universal across all implementations. Use `--help` and `man command` on your instance to verify supported options.

## 18. Common errors and fixes

### `No such file or directory`

Cause: a parent path does not exist or the path is misspelled.

```bash
mkdir -p parent/child/grandchild
```

### `File exists`

Cause: a file/directory already has that name.

```bash
ls -ld path-name
```

Use `mkdir -p` when you intentionally want existing directories to be accepted; it does not turn an existing regular file into a directory.

### `Permission denied`

Cause: your user lacks the required permission on a file or parent directory.

```bash
whoami
ls -ld .
ls -l target
```

Understand the ownership and permissions first. Do not reflexively add `sudo` or use `chmod 777`.

### `Is a directory`

Cause: a command expects a file or a different type of path.

```bash
file path-name
ls -ld path-name
```

### `Directory not empty`

Cause: `rmdir` only removes empty directories.

```bash
find directory-name -maxdepth 2 -print
```

Remove only the intended practice files after inspection.

### `No space left on device`

Check both space and inode availability:

```bash
df -h
df -i
```

### Command not found

The utility may not be installed or may not be in `PATH`.

```bash
command -v tree
echo "$PATH"
```

Use your distribution's package manager only when installation is necessary.

## 19. EC2 practice labs

Run labs inside the dedicated practice directory created in Section 2. Commands below are designed to be repeatable; if a destination already exists, inspect it first.

### Lab 1 — Navigation and creation

```bash
cd ~/linux-practice/02-file-system
pwd
mkdir -p lab1/{docs,logs,backup}
touch lab1/docs/readme.txt
printf 'Linux practice\n' > lab1/docs/readme.txt
find lab1 -print
```

**Goal:** create a directory tree and verify it.

### Lab 2 — Copy, move, rename, read

```bash
cd ~/linux-practice/02-file-system
cp lab1/docs/readme.txt lab1/backup/readme-copy.txt
mv lab1/backup/readme-copy.txt lab1/backup/final.txt
cat lab1/backup/final.txt
ls -l lab1/backup
```

**Goal:** explain the difference between `cp` and `mv`.

### Lab 3 — Search

```bash
cd ~/linux-practice/02-file-system
find lab1 -type f -print
find lab1 -name '*.txt'
grep -n 'Linux' lab1/docs/readme.txt
```

**Goal:** find files by name and text.

### Lab 4 — Metadata and storage

```bash
cd ~/linux-practice/02-file-system
stat lab1/docs/readme.txt
du -sh lab1
df -h .
lsblk -f
```

**Goal:** explain which command reports file metadata, directory usage, filesystem space, and block devices.

### Lab 5 — Archive and extract

```bash
cd ~/linux-practice/02-file-system
tar -czvf lab1.tar.gz lab1
tar -tzvf lab1.tar.gz
mkdir -p lab5-extracted
tar -xzvf lab1.tar.gz -C lab5-extracted
find lab5-extracted -maxdepth 4 -print
```

**Goal:** create, inspect, and extract an archive.

### Optional cleanup of practice files

Only after checking that you are in the intended lab directory:

```bash
cd ~/linux-practice/02-file-system
pwd
find lab5-extracted -maxdepth 3 -print
```

Remove individual lab files only when you are certain you no longer need them. You do not need to clean up the lab to complete this module.

## 20. Interview questions

Try answering these without looking at the notes.

1. What is the Linux filesystem hierarchy?
2. What is the difference between `/` and `/root`?
3. What is the difference between an absolute and a relative path?
4. What do `.`, `..`, and `~` mean?
5. What is the difference between `ls -l` and `ls -la`?
6. What is a hidden file in Linux?
7. What is the difference between `mkdir` and `mkdir -p`?
8. Does `touch` erase existing file contents?
9. What is the difference between `cat`, `less`, `head`, and `tail`?
10. What is the difference between `cp` and `mv`?
11. What is the difference between `rm` and `rmdir`?
12. Why is `rm -rf` dangerous?
13. How do you find all `.log` files under a directory?
14. How do you search for a word inside a text file?
15. What does `stat` display?
16. What is the difference between `du` and `df`?
17. What does `lsblk` display?
18. What is an inode?
19. What is the difference between a hard link and a symbolic link?
20. What is the purpose of `tar`?
21. What do `-c`, `-x`, `-t`, `-z`, and `-f` mean in `tar`?
22. What is a mount point?
23. What is the difference between `>` and `>>`?
24. What does a pipe (`|`) do?
25. What does `chmod 600 file` mean?
26. Why should you avoid practicing destructive commands on `/` or system directories?
27. How do you check free space on an EC2 filesystem?
28. How can you check which filesystem contains `/`?
29. Why can `locate` return outdated results?
30. What should you check before deleting a directory recursively?

### Suggested short answers

1. The directory tree starts at `/`.
2. `/` is the filesystem root; `/root` is the root user's home directory.
3. Absolute starts at `/`; relative starts from the current directory.
4. Current, parent, and home directory.
5. `-la` includes hidden entries.
6. Its name begins with a dot; this does not make it secure.
7. `-p` creates missing parents.
8. No; it normally updates timestamps if it exists.
9. Whole file, paged view, first lines, and last lines.
10. Copy versus move/rename.
11. `rm` removes files; `rmdir` removes empty directories.
12. It can recursively and forcibly delete large amounts of data.
13. `find path -type f -name '*.log'`
14. `grep 'word' file`
15. Metadata, ownership, permissions, size, and timestamps.
16. File/directory usage versus filesystem capacity.
17. Block devices, partitions, and related mount/filesystem information.
18. Filesystem metadata associated with a file.
19. A path-based link versus another name for the same inode.
20. Create/list/extract archives.
21. Create, extract, list, gzip, archive filename.
22. A directory where a filesystem is attached.
23. Overwrite versus append.
24. Passes output into another command.
25. Owner read/write; no group/other permissions.
26. You can break the OS or delete important data.
27. `df -h`
28. `findmnt /` or `df -hT /`
29. Its database may be stale.
30. Verify the current path and inspect exactly what will be removed.

## 21. Quick revision cheat sheet

| Need | Command |
|---|---|
| Current directory | `pwd` |
| Go home | `cd ~` |
| Go parent | `cd ..` |
| List including hidden | `ls -la` |
| Create nested directories | `mkdir -p a/b/c` |
| Create file | `touch file.txt` |
| Read file | `cat file.txt` |
| View long file | `less file.txt` |
| Show first/last lines | `head file.txt`, `tail file.txt` |
| Copy | `cp source destination` |
| Copy directory | `cp -r source destination` |
| Move/rename | `mv source destination` |
| Remove file | `rm -i file.txt` |
| Remove empty directory | `rmdir directory` |
| Find by name | `find . -name '*.txt'` |
| Search text | `grep -n 'word' file.txt` |
| Metadata | `stat file.txt` |
| Directory size | `du -sh directory` |
| Filesystem free space | `df -h` |
| Block devices | `lsblk -f` |
| Archive | `tar -czvf archive.tar.gz directory/` |
| List archive | `tar -tzvf archive.tar.gz` |
| Extract archive | `tar -xzvf archive.tar.gz -C destination/` |
| Show mounts | `findmnt` |
| Read command help | `command --help` |
| Manual | `man command` |

## 22. Completion checklist

Mark each item after you can explain and demonstrate it.

- [ ] I understand the important Linux directories.
- [ ] I can navigate using `pwd`, `cd`, and `ls`.
- [ ] I can create files and directories.
- [ ] I can read files safely.
- [ ] I can copy, move, rename, and remove lab files.
- [ ] I can search by filename and content.
- [ ] I understand file metadata and disk usage.
- [ ] I can explain hard links and symbolic links.
- [ ] I can create and extract a `tar.gz` archive.
- [ ] I understand basic permissions and redirection.
- [ ] I can inspect filesystems and mounts without changing them.
- [ ] I completed all five EC2 practice labs.
- [ ] I answered the interview questions in my own words.

---

## Revision strategy

1. **Day 1:** Paths, `pwd`, `cd`, `ls`, `mkdir`, `touch`.
2. **Day 2:** Reading files, `cp`, `mv`, `rm`, `rmdir`.
3. **Day 3:** `find`, `grep`, `stat`, `du`, `df`.
4. **Day 4:** Links, archives, permissions, redirection.
5. **Day 5:** Repeat the EC2 labs without copying the commands.

**Remember:** Learn what a command does, practice it on sample files, verify the result, and explain it in your own words. Understanding is more valuable than memorizing commands.
