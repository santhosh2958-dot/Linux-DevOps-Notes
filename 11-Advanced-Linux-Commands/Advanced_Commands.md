# Advanced Linux Commands 

### Search for a pattern in a file (case-sensitive)
```bash
grep "pattern" filename.txt
```

### Search for a pattern (case-insensitive)
```bash
grep -i "pattern" filename.txt
```

### Recursively search for a string in all files in a directory
```bash
grep -r "pattern" /path/to/directory
```

### Show line numbers with matched text
```bash
grep -n "pattern" filename.txt
```


### Replace text in a file (does not modify original file)
```bash
sed 's/old/new/' filename.txt
```

### Replace all occurrences in a file
```bash
sed 's/old/new/g' filename.txt
```

### Replace and save changes to the same file (in-place)
```bash
sed -i 's/old/new/g' filename.txt
```

### Print specific fields from a file or command output
```bash
awk '{print $1}' filename.txt
```

### Print lines that match a pattern
```bash
awk '/pattern/ {print}' filename.txt
```

### Securely copy a file to a remote server
```bash
scp file.txt user@remote:/path/to/destination/
```

### Copy a directory to a remote server (with -r flag)
```bash
scp -r folder/ user@remote:/path/
```

### Create a hard link to a file
```bash
ln original.txt hardlink.txt
```

### Create a symbolic (soft) link
```bash
ln -s original.txt symlink.txt
```

---

## ZIP Command (Compress Files)

### Install zip (if not installed)

```bash
sudo yum install zip     # RHEL/CentOS
sudo apt install zip     # Ubuntu/Debian
```

### Create a Zip File

```bash
zip archive.zip file.txt
```

### Zip Multiple Files

```bash
zip archive.zip file1.txt file2.txt
```

### Zip a Directory

```bash
zip -r archive.zip myfolder
```

`-r` = recursive (include all files inside directory)

### Extract Zip File

```bash
unzip archive.zip
```

Extract to specific folder:

```bash
unzip archive.zip -d /path/to/directory
```

## View Zip Content

```bash
unzip -l archive.zip
```

---

## NOHUP Command (Run Process in Background)

### What is nohup?

* `nohup` = No Hang Up
* Runs a command even after logout
* Useful for long-running jobs

### Basic Syntax

```bash
nohup command &
```

Example:

```bash
nohup python script.py &
```

### Output File

By default, output goes to:

```
nohup.out
```

### Redirect Output to Custom File

```bash
nohup python script.py > output.log 2>&1 &
```

Explanation:

* `>` → redirect output
* `2>&1` → redirect errors
* `&` → run in background

### Check Running Background Jobs

```bash
ps -ef | grep script.py
```
---

### Stop Background Process

Find PID:

```bash
ps -ef | grep script.py
```

Kill it:

```bash
kill PID
```

Force kill:

```bash
kill -9 PID
```

---

## FIND Command (Search Files & Directories)

### Basic Syntax

```bash
find /path -option value
```

### Find File by Name

```bash
find /home -name file.txt
```

Case insensitive:

```bash
find /home -iname file.txt
```

---

### Find by File Type

| Type      | Command   |
| --------- | --------- |
| File      | `-type f` |
| Directory | `-type d` |

Example:

```bash
find /home -type f -name "*.txt"
```

### Find by Size

```bash
find /home -size +10M
```

| Symbol | Meaning      |
| ------ | ------------ |
| +      | Greater than |
| -      | Less than    |

Examples:

```bash
find / -size +100M
find / -size -1G
```

### Find by Permission

```bash
find /home -perm 755
```

### Find by Owner

```bash
find /home -user john
```

---
