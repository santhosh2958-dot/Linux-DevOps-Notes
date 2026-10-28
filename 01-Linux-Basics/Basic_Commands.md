# very basic and commonly used Linux commands

### Create a directory
```bash
mkdir folder_name
```

### Create multiple directories at once
```bash
mkdir dir1 dir2 dir3
```

### Create a directory including parent directories
```bash
mkdir -p parent/child
```

### creates a directory tree with multiple subdirectories

```bash
mkdir -p project/{src,bin,docs}
```

### Show current directory path
```bash
pwd
```

### Stay in the current directory (no change)
```bash
cd .
```

### Go up one level back
```bash
cd ..
```

### Go up two levels back
```bash
cd ../..
```

### Delete an empty directory
```bash
rmdir foldername
```

### Delete a directory with all its contents (files + subfolders)
```bash
rm -rf foldername
```

### Create an empty file
```bash
touch filename.txt
```

### Create multiple files at once
```bash
touch file1.txt file2.txt file3.txt
```

### Create a hidden file

```bash
touch .hiddenfile
```

### Create files with brace expansion
```bash
touch file{1..5}.log
```


### Open or create a file in Vim editor
```bash
vim filename.txt
```

### Display contents of a file
```bash
cat filename.txt
```

### Display file with line numbers
```bash
cat -n filename.txt
```

### Delete a single file
```bash
rm filename.txt
```

### Delete multiple files
```bash
rm file1.txt file2.log file3.csv
```

### Delete all files of a type
```bash
rm *.txt
rm *.log
```

### Keep One Directory/File
```bash
rm -rf !(keepdir/file)
```

### Keep Multiple Directories/Files
```bash
rm -rf !(dir1|dir2|dir3)
```

### List files in the current directory
```bash
ls
```

### List files with detailed information
```bash
ls -l
```

### List files with human-readable sizes

```bash
ls -lh
```

### List files by file size (largest first)

```bash
ls -lhS
```

### List files by file size (smallest first)
```bash
ls -lhSr
```

### List files sorted by modification time (latest first)
```bash
ls -lt
```

### List files sorted by modification time (oldest first)
```bash
ls -ltr
```

### List all files including hidden ones
```bash
ls -a
```

### Display text in terminal
```bash
echo "Hello, World"
```

### Write text into a file (overwrites content)
```bash
echo "Hello" > file.txt
```

### Append text to a file
```bash
echo "More text" >> file.txt
```

### Redirect command output to a file
```bash
ls -l > files.txt
```

### Append command output to a file
```bash
ls -l >> files.txt
```

### View output and save to file at the same time
```bash
ls -l | tee output.txt
```

### Append command output to file
```bash
ls -l | tee -a output.txt
```

### Display current date and time
```bash
date
```

### Show the current operating system
```bash
cat /etc/os-release
```

### Show available memory (RAM)
```bash
free -h
```

### Show disk space usage
```bash
df -h
```

### Show disk usage of a specific directory
```bash
du -sh /path/to/directory
```

### Check private/local IP address
```bash
hostname -I
```

### Check public IP address
```bash
curl ifconfig.me
```

### Show CPU information
```bash
lscpu
```

### List previously used commands
```bash
history
```

### Run previous command using arrow keys  
**⬆️ Press the up arrow key** in terminal to access previous commands

### Search through command history  
**Press Ctrl + R** and start typing a previous command

### Clear the terminal screen
```bash
clear
```

### Shortcut to clear the terminal  
**Press Ctrl + L** (same as `clear`)

---
