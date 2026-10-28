# Essential File & Text Commands in Linux

### Copy a file
```bash
cp source.txt destination.txt
```

### Copy a file into a directory
```bash
cp file.txt /path/to/folder/
```

### Copy a directory (recursively)
```bash
cp -r folder1/ folder2/
```

### Move a file
```bash
mv file.txt /path/to/new_location/
```

### Rename a file
```bash
mv oldname.txt newname.txt
```

### Display directory structure as a tree
```bash
tree
```

> If not installed, install with:
```bash
sudo apt install tree
```

### Count lines, words, and characters in a file
```bash
wc file.txt
```

### Count only lines
```bash
wc -l file.txt
```

### View first 10 lines of a file
```bash
head file.txt
```

### View first 20 lines
```bash
head -n 20 file.txt
```

### View last 10 lines of a file
```bash
tail file.txt
```

### View last 20 lines
```bash
tail -n 20 file.txt
```

### Follow real-time changes to a file (like logs)
```bash
tail -f logfile.log
```

---

## File & Directory Permissions in Linux

### Check File Permissions

```bash
ls -l
```

Example output:

```
-rwxr-xr-- 1 john dev 4096 Feb 11 file.txt
```

Breakdown:

```
-rwxr-xr--
│ │  |  │
│ │  |  └── Others (r--)
│ │  └──── Group (r-x)
│ └────── User/Owner (rwx)
└───────── File type (- = file, d = directory)
```

---

### Who is User, Group and Others?

| Type       | Meaning             |
| ---------- | ------------------- |
| User (u)   | Owner of the file   |
| Group (g)  | Users in same group |
| Others (o) | Everyone else       |

---

### Permission Types

| Symbol | Meaning | Value |
| ------ | ------- | ----- |
| r      | Read    | 4     |
| w      | Write   | 2     |
| x      | Execute | 1     |


### Numeric (Octal) Permission Method

Add the values:

| Permission | Value     |
| ---------- | --------- |
| rwx        | 7 (4+2+1) |
| rw-        | 6 (4+2)   |
| r-x        | 5 (4+1)   |
| r--        | 4         |
| ---        | 0         |


### Example: chmod 755

```bash
chmod 755 file.txt
```

Meaning:

```
7 → Owner (rwx)
5 → Group (r-x)
5 → Others (r-x)
```

### Symbolic Method (u g o)

Instead of numbers, use letters:

| Symbol | Meaning |
| ------ | ------- |
| u      | User    |
| g      | Group   |
| o      | Others  |
| a      | All     |


### Add Permission

```bash
chmod u+x file.txt
```
(Add execute to owner)

### Remove Permission

```bash
chmod g-w file.txt
```
(Remove write from group)

### Set Exact Permission

```bash
chmod u=rwx,g=rx,o=r file.txt
```

---

### Directory Permissions Meaning

For directories:

| Permission | Meaning              |
| ---------- | -------------------- |
| r          | List files           |
| w          | Create/Delete files  |
| x          | Enter directory (cd) |

Example:

```bash
chmod 755 mydir
```

---

### Change Owner and Group

### Change Owner

```bash
sudo chown username file.txt
```

### Change Group

```bash
sudo chown :groupname file.txt
```

### Change Owner and Group Together

```bash
sudo chown username:groupname file.txt
```

---

### Change Permissions Recursively

For directory and all inside:

```bash
chmod -R 755 mydir
```

```bash
chown -R username:groupname mydir
```

---

## What is umask?

`umask` sets **default permissions** for new files and directories.

---

## Check Current umask

```bash
umask
```

Example output:

```
0022
```

---

## How umask Works

Default permissions:

| Type      | Default |
| --------- | ------- |
| File      | 666     |
| Directory | 777     |

Then subtract umask value.

Example:

If umask = 022

### For Files:

```
666 - 022 = 644
```

### For Directories:

```
777 - 022 = 755
```

---

## Change umask Temporarily

```bash
umask 0027
```

## Common umask Values

| umask | File Permission | Directory Permission |
| ----- | --------------- | -------------------- |
| 022   | 644             | 755                  |
| 002   | 664             | 775                  |
| 027   | 640             | 750                  |

---

