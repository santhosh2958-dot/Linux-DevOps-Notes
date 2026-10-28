
# 04 - Linux Shell Scripting: Complete Beginner-to-DevOps Guide

Welcome to the **Linux Shell Scripting** learning guide!

This guide starts from the basics and gradually moves towards practical Linux administration, automation, and DevOps projects.

You do not need previous programming experience to start learning.

By the end of this guide, you should be able to write shell scripts to automate repetitive tasks, check server health, back up files, analyse logs, monitor disk usage, and troubleshoot common Linux problems.

---

# Table of Contents

1. [What Is a Shell?](#1-what-is-a-shell)
2. [Why Do We Need a Shell?](#2-why-do-we-need-a-shell)
3. [What Is Bash?](#3-what-is-bash)
4. [Shell vs Terminal vs Bash vs Shell Script](#4-shell-vs-terminal-vs-bash-vs-shell-script)
5. [What Is Shell Scripting?](#5-what-is-shell-scripting)
6. [Where Is Shell Scripting Used in DevOps?](#6-where-is-shell-scripting-used-in-devops)
7. [Check Your Linux Environment](#7-check-your-linux-environment)
8. [Create and Run Your First Script](#8-create-and-run-your-first-script)
9. [Understanding the Shebang](#9-understanding-the-shebang)
10. [Linux Commands Inside Scripts](#10-linux-commands-inside-scripts)
11. [Variables](#11-variables)
12. [Quoting: Single Quotes, Double Quotes and No Quotes](#12-quoting-single-quotes-double-quotes-and-no-quotes)
13. [User Input](#13-user-input)
14. [Command-Line Arguments](#14-command-line-arguments)
15. [Exit Status and Exit Codes](#15-exit-status-and-exit-codes)
16. [Conditions: if, elif and else](#16-conditions-if-elif-and-else)
17. [File and Directory Conditions](#17-file-and-directory-conditions)
18. [Numeric and String Comparisons](#18-numeric-and-string-comparisons)
19. [case Statement](#19-case-statement)
20. [Loops](#20-loops)
21. [Functions](#21-functions)
22. [Arrays](#22-arrays)
23. [Arithmetic Operations](#23-arithmetic-operations)
24. [Command Substitution](#24-command-substitution)
25. [Input, Output, Pipes and Redirection](#25-input-output-pipes-and-redirection)
26. [Useful Text Processing Commands](#26-useful-text-processing-commands)
27. [Read and Process Files](#27-read-and-process-files)
28. [Environment Variables and PATH](#28-environment-variables-and-path)
29. [Error Handling and Safe Scripts](#29-error-handling-and-safe-scripts)
30. [Logging in Shell Scripts](#30-logging-in-shell-scripts)
31. [Practical Script 1: File Backup](#31-practical-script-1-file-backup)
32. [Practical Script 2: Disk Usage Check](#32-practical-script-2-disk-usage-check)
33. [Practical Script 3: Log File Analysis](#33-practical-script-3-log-file-analysis)
34. [Practical Script 4: Service Status Check](#34-practical-script-4-service-status-check)
35. [Practical Script 5: File and Directory Audit](#35-practical-script-5-file-and-directory-audit)
36. [Mini Project: Linux Server Health Checker](#36-mini-project-linux-server-health-checker)
37. [Schedule Scripts Using Cron](#37-schedule-scripts-using-cron)
38. [Debugging and ShellCheck](#38-debugging-and-shellcheck)
39. [Security Best Practices](#39-security-best-practices)
40. [Hands-On Practice Labs](#40-hands-on-practice-labs)
41. [Common Errors and Troubleshooting](#41-common-errors-and-troubleshooting)
42. [Shell Scripting Interview Questions and Answers](#42-shell-scripting-interview-questions-and-answers)
43. [Quick Revision Cheat Sheet](#43-quick-revision-cheat-sheet)
44. [Learning Checklist](#44-learning-checklist)

---

# 1. What Is a Shell?

## 1.1 Understand Linux first

Linux is an operating system.

An operating system manages computer hardware and software resources, such as:

- CPU
- Memory (RAM)
- Disk storage
- Files and directories
- Running programs
- Users and permissions
- Network connections

When working with Linux, we frequently need to communicate with the operating system.

For example, we may need to:

- List files.
- Create a directory.
- Install software.
- Check disk usage.
- Start or stop a service.
- Create users.
- Read system logs.

One way to perform these tasks is through a graphical interface. Another is through commands.

## 1.2 Shell definition

A **shell is a program that accepts commands and interprets them so the operating system can perform the requested tasks.**

For example, when you type:

```bash
ls
```

The shell interprets the command and runs the `ls` program, which lists files and directories.

When you type:

```bash
mkdir practice
```

The command creates a directory named `practice`, assuming you have permission to create it in the current location.

### Simple diagram

```text
You
 |
 | Type a command
 v
Shell (Bash)
 |
 | Interprets command and starts the required program
 v
Linux programs / operating system
 |
 | Performs the operation
 v
Result displayed on the screen
```

**Remember:** The shell is the interface through which you can give commands to Linux. It is not the Linux kernel itself.

## 1.3 Real-life example

Think about ordering food at a restaurant.

- You = the user
- Your order = the command
- Waiter = the shell
- Kitchen = the system that performs the work
- Prepared food = the result

The waiter communicates your request to the kitchen. Similarly, the shell interprets your command and launches the required program.

This is only an analogy; a shell actually processes commands and manages program execution.

---

# 2. Why Do We Need a Shell?

Without a shell, we would need another way to interact with Linux.

The shell is useful because it allows us to work quickly, repeat tasks, combine commands, and automate operations.

## 2.1 Common uses

| Task | Example command |
|---|---|
| List files | `ls -l` |
| Check current directory | `pwd` |
| Create directory | `mkdir reports` |
| Search for a file | `find . -name "*.log"` |
| Check disk usage | `df -h` |
| Check memory | `free -h` |
| View running processes | `ps aux` |
| Check IP information | `ip addr` |
| View a log file | `less /var/log/syslog` |
| Check service status | `systemctl status ssh` |

Some commands require administrator permissions. For example, modifying system configuration may require `sudo`.

## 2.2 Why is this important for DevOps?

DevOps engineers often work with Linux servers, cloud virtual machines, applications, containers, and CI/CD pipelines.

Imagine you manage 50 Linux servers.

You need to check disk usage on every server.

**Manual approach:**

1. Connect to the first server.
2. Run `df -h`.
3. Record the result.
4. Connect to the next server.
5. Repeat the process.

**Automation approach:**

Write one script that checks disk usage and reports the result. Later, you can extend it to check multiple servers securely.

Shell scripting reduces repetitive manual work and makes tasks more consistent.

---

# 3. What Is Bash?

## 3.1 Bash definition

**Bash stands for Bourne Again SHell.**

Bash is a widely used command-line shell on Linux systems.

It understands commands, variables, conditions, loops, functions, and other scripting features.

Bash can be used in two ways:

1. Interactively: Type commands directly into the terminal.
2. Non-interactively: Save commands in a file and execute them as a script.

Example of interactive use:

```bash
whoami
pwd
date
```

Example of a Bash script:

```bash
#!/usr/bin/env bash

echo "Welcome to Linux"
whoami
pwd
date
```

Save the second example as `welcome.sh` and run it as explained later in this guide.

## 3.2 What is the purpose of Bash?

Bash helps you:

- Run Linux commands.
- Store information in variables.
- Make decisions using conditions.
- Repeat tasks using loops.
- Group reusable logic into functions.
- Combine commands using pipes and redirection.
- Automate Linux administration.
- Create scripts for DevOps workflows.

## 3.3 Is Bash the only shell?

No. Linux supports several shells.

| Shell | Description |
|---|---|
| `sh` | Traditional shell interface; on different systems it may refer to different implementations |
| `bash` | Bourne Again SHell; widely used for Linux scripting |
| `zsh` | Interactive shell with additional features |
| `ksh` | KornShell |
| `fish` | Friendly Interactive Shell; has different syntax in several areas |

This guide focuses on **Bash**.

For portable scripts, use POSIX-compatible syntax and a suitable interpreter such as `sh`. For Bash-specific features, explicitly use Bash.

---

# 4. Shell vs Terminal vs Bash vs Shell Script

These terms are related, but they do not mean the same thing.

| Term | Meaning | Example |
|---|---|---|
| Terminal | Application or interface where you type and view text | Ubuntu Terminal, a terminal emulator |
| Shell | Program that interprets commands | Bash, Zsh |
| Bash | A particular shell | `/bin/bash` |
| Shell script | A file containing commands and scripting logic | `backup.sh` |
| Kernel | Core of the operating system; manages resources and hardware interactions | Linux kernel |

### Example

You open Ubuntu Terminal.

Inside it, Bash starts.

You type:

```bash
date
```

Bash launches the `date` command, and the result appears in the terminal.

You then save the following commands in a file:

```bash
#!/usr/bin/env bash
date
whoami
pwd
```

That file is a shell script.

**Memory trick:**

- Terminal = where you type.
- Shell = what interprets your commands.
- Bash = one type of shell.
- Script = saved instructions.
- Kernel = core of the operating system.

---

# 5. What Is Shell Scripting?

Shell scripting means writing multiple shell commands in a file so that they can be executed together.

Instead of manually entering the same commands every day, you can save them in a script and execute the script whenever needed.

## 5.1 Without scripting

Suppose you need to check a server every morning.

You manually run:

```bash
date
hostname
uptime
df -h
free -h
```

You must remember every command and run them one by one.

## 5.2 With scripting

Save the same commands in `server_info.sh`:

```bash
#!/usr/bin/env bash

echo "===== Server Information ====="
date
hostname
uptime
df -h
free -h
```

Run:

```bash
bash server_info.sh
```

The script executes the commands in sequence.

## 5.3 Benefits of shell scripting

1. Automation: Reduce repetitive manual work.
2. Consistency: Perform the same checks in the same way.
3. Speed: Run many commands with one command.
4. Reporting: Collect information into a report file.
5. Scheduling: Execute scripts automatically using cron or other schedulers.
6. Troubleshooting: Collect server and application information.
7. DevOps integration: Use scripts in deployment pipelines and CI/CD jobs.

## 5.4 Limitations

Shell scripting is excellent for command-line automation, but it is not the best choice for every problem.

- Complex data structures can be awkward.
- Large applications may be easier to maintain in Python, Go, or another general-purpose language.
- Scripts must account for permissions, errors, unexpected input, and different Linux distributions.
- A script that works on Ubuntu may require changes on RHEL or Amazon Linux.

Choose the right tool for the job.

---

# 6. Where Is Shell Scripting Used in DevOps?

Shell scripting is useful throughout the DevOps lifecycle.

| Area | Example |
|---|---|
| Linux administration | Create users and directories |
| Monitoring | Check CPU, memory, disk, and uptime |
| Backup | Archive application files |
| Log analysis | Search logs for errors |
| Deployment | Copy files and restart applications |
| CI/CD | Run build, test, and deployment commands |
| Cloud | Automate tasks on AWS EC2 instances |
| Containers | Run commands in Docker build or entrypoint workflows |
| Troubleshooting | Collect diagnostic information |
| Maintenance | Clean old temporary files under a safe retention policy |

A Bash script is one useful automation tool. It does not replace knowledge of Linux, Git, networking, cloud platforms, containers, or CI/CD.

---

# 7. Check Your Linux Environment

Before learning scripts, confirm which shell is available.

## 7.1 Check the current shell

```bash
echo "$SHELL"
```

This usually shows your configured login shell, such as:

```text
/bin/bash
```

To see the shell process associated with the current session, try:

```bash
ps -p "$$" -o comm=
```

The two commands can show different results because the login shell setting and the currently running shell are not always identical.

## 7.2 Check the Bash version

```bash
bash --version
```

## 7.3 Check whether Bash exists

```bash
command -v bash
```

Example output:

```text
/usr/bin/bash
```

The exact path can vary.

## 7.4 Ubuntu setup

On an Ubuntu machine:

```bash
sudo apt update
sudo apt install bash
```

Bash is normally already installed on Ubuntu. You generally do not need to install it separately.

## 7.5 Create a practice folder

```bash
mkdir -p ~/shell-scripting-practice
cd ~/shell-scripting-practice
pwd
```

All examples in the initial practice sections can be created in this folder.

---

# 8. Create and Run Your First Script

Let's create your first Bash script.

## Step 1: Create a file

```bash
nano hello.sh
```

`nano` is a terminal text editor.

## Step 2: Write the script

Enter:

```bash
#!/usr/bin/env bash

echo "Hello, Linux!"
echo "I am learning Shell Scripting."
echo "My username is:"
whoami
```

## Step 3: Save the file

In Nano:

1. Press `Ctrl + O` to save.
2. Press `Enter` to confirm the filename.
3. Press `Ctrl + X` to exit.

## Step 4: Execute it using Bash

```bash
bash hello.sh
```

Expected output resembles:

```text
Hello, Linux!
I am learning Shell Scripting.
My username is:
your-username
```

Your username will be different.

## Step 5: Make the script executable

```bash
chmod +x hello.sh
```

## Step 6: Run the script directly

```bash
./hello.sh
```

### Why do we use `./`?

Linux does not normally search the current directory for executables automatically.

`./hello.sh` means: execute the file named `hello.sh` in the current directory.

### Two common execution methods

| Command | Meaning |
|---|---|
| `bash hello.sh` | Ask Bash to read and execute the script |
| `./hello.sh` | Execute the file directly using its interpreter declaration, if available and valid |

The direct method normally requires execute permission. The `bash hello.sh` method requires the file to be readable but not necessarily executable.

---

# 9. Understanding the Shebang

A shebang is the first line of many executable scripts.

Example:

```bash
#!/usr/bin/env bash
```

It tells the operating system to use Bash to interpret the script when the script is executed directly.

### Breakdown

- `#!` identifies the interpreter declaration.
- `/usr/bin/env` locates a program using the environment's `PATH`.
- `bash` specifies the interpreter to locate.

Another common form is:

```bash
#!/bin/bash
```

Use a shebang that matches your target environment.

### Example

```bash
#!/usr/bin/env bash

echo "This script is executed by Bash."
```

**Important:** A shebang does not install Bash and does not grant administrator privileges.

---

# 10. Linux Commands Inside Scripts

A shell script can run the same Linux commands you normally type into a terminal.

Example:

```bash
#!/usr/bin/env bash

echo "Current date:"
date

echo "Current user:"
whoami

echo "Current directory:"
pwd

echo "Hostname:"
hostname

echo "System uptime:"
uptime
```

## Important commands for DevOps learners

| Command | Purpose |
|---|---|
| `pwd` | Display current directory |
| `ls -lah` | List files with details, including hidden entries |
| `mkdir -p` | Create directories, including missing parent directories |
| `cp` | Copy files |
| `mv` | Move or rename files |
| `rm` | Remove files |
| `cat` | Print file contents |
| `less` | View files interactively |
| `head` | Show the beginning of a file |
| `tail` | Show the end of a file |
| `grep` | Search text |
| `find` | Find files and directories |
| `df -h` | Show filesystem space |
| `du -sh` | Estimate disk usage of a directory |
| `free -h` | Show memory information |
| `ps` | Display process information |
| `systemctl` | Manage or inspect systemd services |
| `journalctl` | Read systemd journal logs |

---

# 11. Variables

A variable stores a value that you can reuse.

## 11.1 Create a variable

```bash
name="Santhosh"
```

Display it:

```bash
echo "$name"
```

Output:

```text
Santhosh
```

## 11.2 Important variable rules

Correct:

```bash
name="Santhosh"
age=30
```

Incorrect:

```bash
name = "Santhosh"
```

Do not put spaces around `=` when assigning a Bash variable.

## 11.3 Use variables in a script

```bash
#!/usr/bin/env bash

name="Santhosh"
course="DevOps"

echo "Name: $name"
echo "Course: $course"
```

## 11.4 Curly braces

```bash
name="Linux"

echo "${name}Scripting"
```

Output:

```text
LinuxScripting
```

Curly braces make the variable boundary clear.

## 11.5 Read-only variables

```bash
readonly company="ExampleCompany"
```

A read-only variable cannot be reassigned during the shell session.

## 11.6 Special variables

| Variable | Meaning |
|---|---|
| `$0` | Script name or invocation name |
| `$1` | First positional argument |
| `$2` | Second positional argument |
| `$#` | Number of positional arguments |
| `$@` | All positional arguments as separate words when quoted as `"$@"` |
| `$*` | All positional arguments; quoting behaves differently from `"$@"` |
| `$?` | Exit status of the most recently completed command |
| `$$` | Process ID of the current shell |
| `$!` | Process ID of the most recently started background job |

Example:

```bash
#!/usr/bin/env bash

echo "Script name: $0"
echo "First argument: $1"
echo "Number of arguments: $#"
```

Run:

```bash
bash args.sh hello world
```

The first argument is `hello`, and the total number of arguments is `2`.

---

# 12. Quoting: Single Quotes, Double Quotes and No Quotes

Quoting is important because Bash expands variables and interprets special characters.

## 12.1 Double quotes

Double quotes allow variable and command substitutions while protecting most whitespace and special characters from word splitting and filename expansion.

```bash
name="Linux"

echo "I am learning $name"
```

Output:

```text
I am learning Linux
```

## 12.2 Single quotes

Single quotes preserve the literal characters inside them.

```bash
name="Linux"

echo 'I am learning $name'
```

Output:

```text
I am learning $name
```

The variable is not expanded.

## 12.3 No quotes

```bash
name="Linux Scripting"

echo $name
```

This simple example may display the expected text, but unquoted expansions can undergo word splitting and filename expansion.

Prefer:

```bash
echo "$name"
```

## 12.4 Best practice

Quote variable expansions unless you specifically need word splitting or filename expansion.

Prefer:

```bash
cp "$source_file" "$destination_file"
```

Avoid:

```bash
cp $source_file $destination_file
```

The quoted version handles paths containing spaces more reliably.

---

# 13. User Input

Use `read` to collect information from the user.

## Example

```bash
#!/usr/bin/env bash

read -r -p "Enter your name: " name

echo "Welcome, $name!"
```

### Explanation

- `read` reads input.
- `-r` prevents backslashes from being treated specially.
- `-p` displays a prompt.
- `name` stores the input.

## Example: Add two numbers

```bash
#!/usr/bin/env bash

read -r -p "Enter the first number: " num1
read -r -p "Enter the second number: " num2

if [[ "$num1" =~ ^[0-9]+$ && "$num2" =~ ^[0-9]+$ ]]; then
    echo "Sum: $((num1 + num2))"
else
    echo "Please enter non-negative whole numbers."
    exit 1
fi
```

This example validates that the input consists of non-negative whole numbers before arithmetic expansion.

---

# 14. Command-Line Arguments

Arguments let users pass values to a script when running it.

## Example: Greeting script

Create `greet.sh`:

```bash
#!/usr/bin/env bash

if [[ $# -lt 1 ]]; then
    echo "Usage: $0 NAME"
    exit 1
fi

name="$1"

echo "Hello, $name!"
```

Run:

```bash
bash greet.sh Santhosh
```

Output:

```text
Hello, Santhosh!
```

## Example with two arguments

```bash
#!/usr/bin/env bash

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 SOURCE DESTINATION"
    exit 1
fi

source_file="$1"
destination_file="$2"

cp -- "$source_file" "$destination_file"
```

Run:

```bash
bash copy.sh report.txt report-copy.txt
```

This copies a file if the source exists and the destination is writable.

### Important argument concepts

- `$1`: first argument.
- `$2`: second argument.
- `$#`: argument count.
- `"$@"`: preserve each argument as a separate item.

When forwarding arguments to another command or function, `"$@"` is generally safer than unquoted `$@`.

---

# 15. Exit Status and Exit Codes

Every command returns an exit status.

Conventionally:

- `0` means success.
- A non-zero status indicates failure or another condition.

## Example

```bash
ls /tmp
echo "$?"
```

If `ls` succeeds, the exit status is normally `0`.

Now try:

```bash
ls /directory-that-does-not-exist
echo "$?"
```

The status will normally be non-zero.

## Use an exit code in a script

```bash
#!/usr/bin/env bash

if [[ -f "/etc/passwd" ]]; then
    echo "File exists."
    exit 0
else
    echo "File not found."
    exit 1
fi
```

### Important rule

`$?` refers to the most recently completed command's status. If you run `echo` before checking it, you will usually see the status of `echo` instead.

Correct:

```bash
some_command
status=$?

echo "Command status: $status"
```

Even better for many checks:

```bash
if some_command; then
    echo "Command succeeded."
else
    echo "Command failed."
fi
```

---

# 16. Conditions: if, elif and else

Conditions allow a script to make decisions.

## 16.1 Basic if statement

```bash
#!/usr/bin/env bash

number=10

if [[ "$number" -gt 5 ]]; then
    echo "Number is greater than 5."
fi
```

## 16.2 if and else

```bash
#!/usr/bin/env bash

number=3

if [[ "$number" -gt 5 ]]; then
    echo "Number is greater than 5."
else
    echo "Number is 5 or less."
fi
```

## 16.3 if, elif and else

```bash
#!/usr/bin/env bash

read -r -p "Enter disk usage percentage: " usage

if [[ ! "$usage" =~ ^[0-9]+$ ]] || (( usage > 100 )); then
    echo "Enter a whole number from 0 to 100."
elif (( usage >= 80 )); then
    echo "WARNING: Disk usage is high."
else
    echo "Disk usage is within the example threshold."
fi
```

This script validates a percentage and then checks whether it meets the warning threshold.

## 16.4 Syntax to remember

```bash
if condition; then
    commands
elif another_condition; then
    commands
else
    commands
fi
```

**Memory trick:** `if` starts the decision; `fi` closes it.

---

# 17. File and Directory Conditions

Bash has tests for files and directories.

| Test | Meaning |
|---|---|
| `-e FILE` | Path exists |
| `-f FILE` | Path is a regular file |
| `-d PATH` | Path is a directory |
| `-r FILE` | File is readable |
| `-w FILE` | File is writable |
| `-x FILE` | File is executable or searchable, depending on file type |
| `-s FILE` | File exists and has a size greater than zero |
| `-L PATH` | Path is a symbolic link |

## Example: Check a file

```bash
#!/usr/bin/env bash

file="/etc/passwd"

if [[ -f "$file" ]]; then
    echo "$file exists and is a regular file."
else
    echo "File not found."
fi
```

## Example: Check a directory

```bash
#!/usr/bin/env bash

directory="/var/log"

if [[ -d "$directory" ]]; then
    echo "Log directory exists."
else
    echo "Log directory is missing."
fi
```

## Example: Check if a file is empty

```bash
if [[ -s "application.log" ]]; then
    echo "Log file has content."
else
    echo "Log file is missing or empty."
fi
```

Note that `-s` does not distinguish between a missing file and an existing empty file. Use `-e` and `-s` separately if you need that distinction.

---

# 18. Numeric and String Comparisons

## 18.1 Numeric comparisons

Inside `[[ ... ]]`, use the following operators for integer comparisons:

| Operator | Meaning |
|---|---|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |
| `-lt` | Less than |
| `-le` | Less than or equal |

Example:

```bash
a=10
b=5

if [[ "$a" -gt "$b" ]]; then
    echo "a is greater than b."
fi
```

Bash also supports arithmetic conditions:

```bash
if (( a > b )); then
    echo "a is greater than b."
fi
```

## 18.2 String comparisons

```bash
name="Linux"

if [[ "$name" == "Linux" ]]; then
    echo "Match found."
fi
```

Useful string tests:

```bash
[[ -z "$value" ]]   # True when the string is empty
[[ -n "$value" ]]   # True when the string is non-empty
[[ "$a" == "$b" ]]  # Strings are equal
[[ "$a" != "$b" ]]  # Strings are different
```

Use `[[ ... ]]` for Bash conditions. It handles many common quoting situations more safely than the older `[ ... ]` form.

---

# 19. case Statement

Use `case` when a script must choose from several options.

```bash
#!/usr/bin/env bash

read -r -p "Enter an option (start/stop/status): " action

case "$action" in
    start)
        echo "Starting application..."
        ;;
    stop)
        echo "Stopping application..."
        ;;
    status)
        echo "Checking application status..."
        ;;
    *)
        echo "Invalid option."
        echo "Usage: start | stop | status"
        exit 1
        ;;
esac
```

### Important symbols

- `case` starts the selection.
- `)` separates a pattern from its commands.
- `;;` ends a case branch.
- `*` matches any value.
- `esac` closes the statement.

**Memory trick:** `case` spelled backwards is `esac`.

---

# 20. Loops

Loops repeat commands.

## 20.1 for loop

```bash
#!/usr/bin/env bash

for name in Linux Git Docker
do
    echo "Learning: $name"
done
```

## 20.2 Loop through numbers

```bash
for number in {1..5}; do
    echo "Number: $number"
done
```

Brace expansion such as `{1..5}` is a Bash feature.

## 20.3 while loop

```bash
#!/usr/bin/env bash

count=1

while (( count <= 5 )); do
    echo "Count: $count"
    ((count += 1))
done
```

## 20.4 Loop through files

```bash
#!/usr/bin/env bash

for file in ./*.log; do
    [[ -f "$file" ]] || continue
    echo "Log file: $file"
done
```

The `[[ -f "$file" ]]` check prevents an unmatched `./*.log` pattern from being treated as a real file.

## 20.5 break and continue

- `break`: exit the loop.
- `continue`: skip to the next iteration.

Example:

```bash
for number in {1..5}; do
    if (( number == 3 )); then
        continue
    fi

    echo "$number"
done
```

Output:

```text
1
2
4
5
```

---

# 21. Functions

A function groups reusable commands under a name.

## Example

```bash
#!/usr/bin/env bash

greet() {
    echo "Hello from a function!"
}

greet
greet
```

The function can be called multiple times.

## Function with an argument

```bash
#!/usr/bin/env bash

greet() {
    local name="$1"
    echo "Hello, $name!"
}

greet "Santhosh"
greet "DevOps learner"
```

## Return status

Bash functions can return an integer status using `return`.

```bash
check_file() {
    [[ -f "$1" ]]
}

if check_file "/etc/passwd"; then
    echo "File exists."
else
    echo "File not found."
fi
```

A Bash `return` status is an integer from 0 to 255. It is not designed to return arbitrary text.

To produce text, use standard output:

```bash
get_hostname() {
    hostname
}

server_name="$(get_hostname)"
echo "$server_name"
```

---

# 22. Arrays

Bash supports indexed arrays.

## Create an array

```bash
tools=("Linux" "Git" "Docker" "Kubernetes")
```

## Access an item

```bash
echo "${tools[0]}"
```

Bash indexed arrays use zero-based indexing, so the first item is index `0`.

## Display all items

```bash
printf '%s\n' "${tools[@]}"
```

## Display array length

```bash
echo "${#tools[@]}"
```

## Loop through an array

```bash
for tool in "${tools[@]}"; do
    echo "Learning: $tool"
done
```

Keep `"${tools[@]}"` quoted so each array element is preserved as a separate item.

---

# 23. Arithmetic Operations

Bash supports integer arithmetic.

```bash
a=10
b=3

echo "$((a + b))"
echo "$((a - b))"
echo "$((a * b))"
echo "$((a / b))"
echo "$((a % b))"
```

Output:

```text
13
7
30
3
1
```

Integer division truncates the fractional part. Bash arithmetic does not provide ordinary floating-point arithmetic.

## Increment a variable

```bash
count=1
((count += 1))

echo "$count"
```

## Important caution

Do not assume user-provided text is a valid number. Validate input before using it in arithmetic operations.

---

# 24. Command Substitution

Command substitution captures a command's output and places it into a variable or another command.

Recommended syntax:

```bash
current_date="$(date)"
hostname_value="$(hostname)"

echo "$current_date"
echo "$hostname_value"
```

Older syntax:

```bash
current_date=`date`
```

Prefer `$(...)` because it is easier to read and supports nesting more clearly.

## Example: Count files

```bash
file_count="$(find . -maxdepth 1 -type f | wc -l)"

echo "Files in the current directory: $file_count"
```

Command substitution removes trailing newline characters from the captured output.

---

# 25. Input, Output, Pipes and Redirection

## 25.1 Standard streams

Linux commands commonly use three standard streams.

| Stream | Number | Purpose |
|---|---:|---|
| Standard input | `0` | Input to a command |
| Standard output | `1` | Normal command output |
| Standard error | `2` | Error messages |

## 25.2 Redirect output to a file

```bash
date > date.txt
```

`>` creates or overwrites the destination file.

## 25.3 Append output

```bash
date >> date.txt
```

`>>` appends to a file rather than overwriting its existing content.

## 25.4 Redirect errors

```bash
ls /directory-that-does-not-exist 2> errors.log
```

Error output is redirected to `errors.log`.

## 25.5 Redirect both output streams

```bash
some_command > command.log 2>&1
```

This redirects standard output to `command.log` and then sends standard error to the same destination.

In Bash, the following is another common form:

```bash
some_command &> command.log
```

## 25.6 Pipes

A pipe sends the standard output of one command to the standard input of another.

```bash
ps aux | grep nginx
```

This searches the process listing for text matching `nginx`. The output may also contain the `grep` process itself.

## 25.7 Useful examples

```bash
df -h > disk-report.txt
```

Save disk usage output.

```bash
grep -i "error" application.log > errors.txt
```

Save matching error lines.

```bash
cat application.log | wc -l
```

Count lines in a file, although `wc -l application.log` is simpler for this task.

---

# 26. Useful Text Processing Commands

These commands are commonly used in scripts.

## grep: Search text

```bash
grep "ERROR" application.log
```

Case-insensitive search:

```bash
grep -i "error" application.log
```

Count matching lines:

```bash
grep -ic "error" application.log
```

Show line numbers:

```bash
grep -in "error" application.log
```

## cut: Select fields

```bash
cut -d: -f1 /etc/passwd
```

This prints the first colon-separated field, commonly usernames.

## sort: Sort lines

```bash
sort names.txt
```

## uniq: Remove adjacent duplicate lines

```bash
sort names.txt | uniq
```

`uniq` works on adjacent matching lines, so sorting first is useful when you want to remove duplicates from an entire text list.

Count repeated lines:

```bash
sort names.txt | uniq -c
```

## wc: Count

```bash
wc -l application.log
wc -w report.txt
wc -c report.txt
```

These count lines, words, and bytes respectively.

## awk: Process columns

```bash
awk '{print $1}' access.log
```

This prints the first whitespace-separated field from each line.

## sed: Transform text

```bash
sed 's/old/new/g' file.txt
```

This prints text with replacements; it does not modify the original file unless you use an in-place option.

Be careful with in-place edits, especially on important configuration files.

---

# 27. Read and Process Files

A robust way to process a text file line by line is:

```bash
#!/usr/bin/env bash

file="names.txt"

if [[ ! -f "$file" ]]; then
    echo "File not found: $file" >&2
    exit 1
fi

while IFS= read -r line || [[ -n "$line" ]]; do
    printf 'Line: %s\n' "$line"
done < "$file"
```

### Why use `IFS= read -r`?

- `IFS=` prevents leading and trailing whitespace from being trimmed by the `read` operation.
- `-r` prevents backslashes from being interpreted as escape characters.
- `|| [[ -n "$line" ]]` allows the final line to be processed even if the file does not end with a newline.

## Search all `.log` files

```bash
find . -type f -name "*.log" -print
```

## Search for errors in log files

```bash
find . -type f -name "*.log" -exec grep -inH "error" {} +
```

This finds `.log` files and searches them for `error`, showing filenames and line numbers.

---

# 28. Environment Variables and PATH

Environment variables provide information to processes.

Examples include:

```bash
echo "$HOME"
echo "$USER"
echo "$PATH"
echo "$SHELL"
```

Common variables:

| Variable | Purpose |
|---|---|
| `HOME` | User's home directory |
| `USER` | Commonly contains the current username |
| `PATH` | Directories searched for executable commands |
| `SHELL` | Usually the configured login shell |
| `PWD` | Current working directory in the shell |

## Shell variable vs environment variable

A shell variable is available within the current shell.

```bash
course="DevOps"
```

To pass it to child processes, export it:

```bash
export course
```

Or assign and export together:

```bash
export course="DevOps"
```

## Add a script directory to PATH temporarily

```bash
export PATH="$HOME/bin:$PATH"
```

This adds `~/bin` to the front of `PATH` for the current shell and its child processes.

Do not add untrusted directories to `PATH`. Doing so can cause an unintended or malicious executable to run.

---

# 29. Error Handling and Safe Scripts

Reliable scripts must account for failures.

## 29.1 Check command results

```bash
if cp -- source.txt destination.txt; then
    echo "Copy completed."
else
    echo "Copy failed." >&2
    exit 1
fi
```

## 29.2 Understand `set -euo pipefail`

A common Bash script header is:

```bash
#!/usr/bin/env bash
set -euo pipefail
```

These options mean:

- `-e`: usually exit when a simple command fails in contexts where `errexit` applies.
- `-u`: treat many references to unset variables as errors.
- `pipefail`: make a pipeline fail if a command in the pipeline fails, rather than reporting only the last command's status.

### Important limitation

`set -e` has exceptions and context-dependent behavior, including conditions, parts of `&&` and `||` lists, and pipelines. It is not a complete replacement for explicit error handling.

Use clear checks around important operations.

## 29.3 Use a temporary directory safely

```bash
#!/usr/bin/env bash
set -euo pipefail

tmp_dir="$(mktemp -d)"
trap 'rm -rf -- "$tmp_dir"' EXIT

echo "Temporary directory: $tmp_dir"
```

`mktemp -d` creates a unique temporary directory. The `trap` removes that directory when the script exits.

Only use automatic cleanup on paths created by your script. Never construct a dangerous cleanup path from unchecked user input.

## 29.4 Validate arguments

```bash
if [[ $# -ne 1 ]]; then
    echo "Usage: $0 DIRECTORY" >&2
    exit 1
fi

directory="$1"

if [[ ! -d "$directory" ]]; then
    echo "Not a directory: $directory" >&2
    exit 1
fi
```

## 29.5 Use meaningful error messages

Prefer:

```bash
echo "ERROR: Backup source does not exist." >&2
```

over a vague message such as:

```bash
echo "Something went wrong."
```

---

# 30. Logging in Shell Scripts

Logs help you understand what a script did and why it failed.

Example:

```bash
#!/usr/bin/env bash
set -u

LOG_FILE="$HOME/shell-script.log"

log_info() {
    printf '[INFO] %s - %s\n' "$(date '+%Y-%m-%d %H:%M:%S')" "$*" \
        | tee -a "$LOG_FILE"
}

log_error() {
    printf '[ERROR] %s - %s\n' "$(date '+%Y-%m-%d %H:%M:%S')" "$*" \
        | tee -a "$LOG_FILE" >&2
}

log_info "Script started."
log_info "Checking server information."
log_error "This is an example error message."
```

The messages are displayed on the terminal and appended to the log file.

For production scripts, also think about log rotation, file permissions, disk space, and whether sensitive information might be recorded.

---

# 31. Practical Script 1: File Backup

## Objective

Create a timestamped archive of a directory.

This is useful when backing up application configuration, website files, or small project folders.

## Script: `backup.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 SOURCE_DIRECTORY BACKUP_DIRECTORY" >&2
    exit 1
fi

source_dir="$1"
backup_dir="$2"

if [[ ! -d "$source_dir" ]]; then
    echo "ERROR: Source directory does not exist: $source_dir" >&2
    exit 1
fi

mkdir -p -- "$backup_dir"

timestamp="$(date '+%Y%m%d_%H%M%S')"
archive="$backup_dir/backup_$timestamp.tar.gz"

tar -czf "$archive" -C "$(dirname "$source_dir")" "$(basename "$source_dir")"

echo "Backup completed successfully."
echo "Archive: $archive"
```

## Understand the commands

| Command or option | Meaning |
|---|---|
| `tar` | Creates or extracts archives |
| `-c` | Create archive |
| `-z` | Compress using gzip |
| `-f` | Specify archive filename |
| `-C` | Change directory before adding files |
| `date` | Generate timestamp |
| `mkdir -p` | Create the destination directory if needed |

## Practice

Create test files:

```bash
mkdir -p ~/backup-practice/source
echo "Application configuration" > ~/backup-practice/source/config.txt
echo "Important data" > ~/backup-practice/source/data.txt
```

Save the script as `backup.sh`, then run:

```bash
bash backup.sh ~/backup-practice/source ~/backup-practice/backups
```

List the resulting archives:

```bash
ls -lh ~/backup-practice/backups
```

Inspect the contents:

```bash
tar -tzf ~/backup-practice/backups/backup_TIMESTAMP.tar.gz
```

Replace `backup_TIMESTAMP.tar.gz` with the actual archive name.

### Production considerations

This is a learning example, not a complete enterprise backup solution.

A production backup process should also consider:

- Whether files are changing during the backup.
- Backup retention and cleanup.
- Available storage space.
- File ownership and permissions.
- Encryption and secure storage.
- Restore testing.
- Off-server copies.
- Monitoring and failure alerts.

A backup is not proven useful until you have successfully tested restoration.

---

# 32. Practical Script 2: Disk Usage Check

## Objective

Check filesystem usage and display a warning when usage reaches a configured threshold.

## Script: `disk_check.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

THRESHOLD=80

echo "===== Disk Usage Report ====="
date
echo

df -P -h

echo
echo "===== Filesystems at or Above ${THRESHOLD}% ====="

df -P -k | awk -v threshold="$THRESHOLD" '
    NR == 1 { next }
    {
        usage = $5
        gsub(/%/, "", usage)

        if (usage ~ /^[0-9]+$/ && usage >= threshold) {
            print "WARNING:", $0
        }
    }
'
```

## Explanation

- `df` reports filesystem space.
- `-P` requests a predictable, single-line-per-filesystem format.
- `-h` displays human-readable sizes.
- `-k` displays sizes in kilobytes.
- `awk` processes the report.
- `gsub()` removes the `%` symbol.
- `threshold` is passed to `awk` using `-v`.

The script prints a warning for filesystems at or above 80% usage.

Some systems use virtual or temporary filesystems that may not be important to alert on. Production monitoring should define which filesystems matter and how to handle network, container, and pseudo-filesystems.

## Run it

```bash
chmod +x disk_check.sh
./disk_check.sh
```

## Practice

Change:

```bash
THRESHOLD=80
```

to:

```bash
THRESHOLD=50
```

Run the script again and compare the output.

Remember that a lower threshold is useful for testing but may generate warnings on ordinary systems.

---

# 33. Practical Script 3: Log File Analysis

## Objective

Count lines containing `ERROR` and `WARNING` in an application log.

## Step 1: Create a sample log

```bash
cat > application.log <<'EOF'
INFO Application started
INFO Connected to database
WARNING Disk usage is increasing
ERROR Database connection failed
INFO Retrying database connection
ERROR Request processing failed
WARNING Response time is high
INFO Application stopped
EOF
```

## Step 2: Create the script

Save as `log_analysis.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

if [[ $# -ne 1 ]]; then
    echo "Usage: $0 LOG_FILE" >&2
    exit 1
fi

log_file="$1"

if [[ ! -f "$log_file" ]]; then
    echo "ERROR: Log file not found: $log_file" >&2
    exit 1
fi

echo "===== Log Analysis ====="
echo "File: $log_file"
echo "Total lines: $(wc -l < "$log_file")"
echo "ERROR lines: $(grep -ic 'ERROR' "$log_file" || true)"
echo "WARNING lines: $(grep -ic 'WARNING' "$log_file" || true)"

echo
echo "===== Error Details ====="
grep -in 'ERROR' "$log_file" || true

echo
echo "===== Warning Details ====="
grep -in 'WARNING' "$log_file" || true
```

## Step 3: Run the script

```bash
bash log_analysis.sh application.log
```

## Why is `|| true` used?

`grep` normally returns:

- `0` when it finds a match.
- `1` when it finds no matches.
- A larger non-zero status for certain errors.

Here, no matches should not prevent the report from continuing. However, `|| true` also masks other `grep` failures in these particular expressions, so a production version should handle missing files and read errors separately when necessary.

## DevOps use case

This approach can be extended to:

- Search application errors.
- Count failed requests.
- Identify repeated warnings.
- Extract timestamps for investigation.
- Produce a daily summary report.

Real log formats differ, so learn the application's logging format before writing production filters.

---

# 34. Practical Script 4: Service Status Check

## Objective

Check whether a systemd service is active.

This example checks SSH, but the service name can vary between Linux distributions.

## Script: `service_check.sh`

```bash
#!/usr/bin/env bash
set -u

if [[ $# -ne 1 ]]; then
    echo "Usage: $0 SERVICE_NAME" >&2
    exit 1
fi

service_name="$1"

if ! command -v systemctl >/dev/null 2>&1; then
    echo "ERROR: systemctl is not available on this system." >&2
    exit 2
fi

if systemctl is-active --quiet "$service_name"; then
    echo "OK: Service '$service_name' is active."
else
    echo "WARNING: Service '$service_name' is not active."
    echo "Check details using: systemctl status '$service_name'"
    exit 1
fi
```

## Run it

```bash
bash service_check.sh ssh
```

On Ubuntu, the SSH service is commonly named `ssh`. On some RHEL-based systems, it is commonly named `sshd`.

Try:

```bash
bash service_check.sh ssh
```

or:

```bash
bash service_check.sh sshd
```

Use the service name appropriate to your machine.

## Useful troubleshooting commands

```bash
systemctl status ssh
```

```bash
systemctl is-enabled ssh
```

```bash
journalctl -u ssh --since "1 hour ago"
```

### Important distinction

- `systemctl is-active` checks the current active state.
- `systemctl is-enabled` checks whether the service is configured to start automatically.
- A service being enabled does not guarantee it is currently running.

Do not automatically restart a production service simply because a check fails. Investigate the cause and follow the relevant change procedure.

---

# 35. Practical Script 5: File and Directory Audit

## Objective

Generate a simple report of regular files in a specified directory.

## Script: `file_audit.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail

if [[ $# -ne 1 ]]; then
    echo "Usage: $0 DIRECTORY" >&2
    exit 1
fi

target_dir="$1"

if [[ ! -d "$target_dir" ]]; then
    echo "ERROR: Directory does not exist: $target_dir" >&2
    exit 1
fi

echo "===== File Audit ====="
echo "Directory: $target_dir"
echo "Generated: $(date)"
echo

find "$target_dir" -type f -print
echo

echo "Total regular files:"
find "$target_dir" -type f -printf '.' | wc -c

echo
echo "Files larger than 10 MB:"
find "$target_dir" -type f -size +10M -print
```

## Run it

```bash
bash file_audit.sh ~/shell-scripting-practice
```

### Important portability note

The `find -printf` option is available in GNU `find`, commonly installed on Ubuntu and many RHEL-based Linux distributions. It is not part of every implementation of `find`.

For portable scripting, use alternatives appropriate to your target operating system.

---

# 36. Mini Project: Linux Server Health Checker

## Project objective

Create a single Bash script that generates a report about a Linux server.

This mini project combines variables, functions, commands, conditions, logging, and redirection.

## What will it check?

1. Hostname.
2. Current date and time.
3. System uptime.
4. CPU information.
5. Memory usage.
6. Filesystem usage.
7. Whether a selected service is active.
8. A report saved to a file.

## Step 1: Create the project directory

```bash
mkdir -p ~/server-health-checker
cd ~/server-health-checker
```

## Step 2: Create the script

Create `health_check.sh`:

```bash
#!/usr/bin/env bash
set -u

REPORT_DIR="$HOME/server-health-checker/reports"
TIMESTAMP="$(date '+%Y%m%d_%H%M%S')"
REPORT_FILE="$REPORT_DIR/health_report_$TIMESTAMP.txt"

mkdir -p -- "$REPORT_DIR"

# Send report output to the terminal and the report file.
exec > >(tee -a "$REPORT_FILE") 2>&1

print_section() {
    printf '\n===== %s =====\n' "$1"
}

check_service() {
    local service_name="$1"

    if ! command -v systemctl >/dev/null 2>&1; then
        echo "systemctl is unavailable; service check skipped."
        return 0
    fi

    if systemctl is-active --quiet "$service_name"; then
        echo "Service '$service_name': ACTIVE"
    else
        echo "Service '$service_name': NOT ACTIVE"
    fi
}

print_section "SERVER HEALTH REPORT"
echo "Generated: $(date)"
echo "Hostname: $(hostname)"
echo "User: $(whoami)"

print_section "UPTIME"
uptime

print_section "CPU INFORMATION"
if [[ -r /proc/cpuinfo ]]; then
    grep -m 1 'model name' /proc/cpuinfo || true
else
    echo "CPU information is unavailable."
fi

print_section "MEMORY USAGE"
free -h

print_section "FILESYSTEM USAGE"
df -h

print_section "SERVICE STATUS"
service_name="${1:-ssh}"
check_service "$service_name"

print_section "REPORT COMPLETE"
echo "Report saved to: $REPORT_FILE"
```

## Step 3: Run the script

```bash
chmod +x health_check.sh
./health_check.sh ssh
```

On a system where the service is named `sshd`, use:

```bash
./health_check.sh sshd
```

## Step 4: Read the report

```bash
ls -lh ~/server-health-checker/reports
```

Then read the report:

```bash
cat ~/server-health-checker/reports/health_report_YYYYMMDD_HHMMSS.txt
```

Replace the filename with the actual report name.

## Step 5: Improve the project

Try implementing these enhancements yourself:

- [ ] Add a configurable disk-usage threshold.
- [ ] Print a warning when memory usage is above a chosen threshold.
- [ ] Check whether important application processes are running.
- [ ] Include the operating system name and version.
- [ ] Save a separate error log.
- [ ] Return a non-zero exit status if a critical check fails.
- [ ] Add an optional output directory argument.
- [ ] Schedule the script to run daily using cron.
- [ ] Document how to run it on Ubuntu and Amazon Linux.
- [ ] Add a README explaining the project, setup, sample output, and limitations.

## Project limitations

This is a learning project, not a complete monitoring system.

- It does not send alerts.
- It does not monitor continuously.
- It does not automatically repair services.
- It does not calculate every metric needed for production monitoring.
- Some commands and service names differ across distributions.
- The script reports information available to the user running it; some information may require additional permissions.

For real production monitoring, consider dedicated monitoring and alerting tools as your needs grow.

---

# 37. Schedule Scripts Using Cron

Cron is a scheduler that runs commands at configured times on many Linux systems.

## 37.1 Open your crontab

```bash
crontab -e
```

## 37.2 Cron syntax

```text
minute hour day-of-month month day-of-week command
```

Example:

```text
0 9 * * * /home/youruser/server-health-checker/health_check.sh ssh
```

This runs the script at 09:00 every day, using the specified absolute path.

Replace `/home/youruser` with your actual home directory.

## 37.3 Run a script every five minutes

```text
*/5 * * * * /home/youruser/server-health-checker/health_check.sh ssh
```

## 37.4 Redirect cron output

```text
*/5 * * * * /home/youruser/server-health-checker/health_check.sh ssh >> /home/youruser/health-check-cron.log 2>&1
```

This appends standard output and error output to a log file.

If the script already writes its own report, decide whether additional cron logging is useful.

## 37.5 Important cron considerations

- Use absolute paths for scripts and important files.
- Make sure the script is executable or invoke it using Bash.
- Set required environment variables explicitly.
- Do not assume cron uses the same `PATH` as your interactive terminal.
- Ensure the script's user has the necessary permissions.
- Check the cron service and system logs if a scheduled task does not run.
- Protect report and log files from unauthorized access.

Cron service names and log locations vary by distribution.

---

# 38. Debugging and ShellCheck

## 38.1 Run Bash syntax checks

```bash
bash -n script.sh
```

This checks syntax without executing the script.

## 38.2 Trace execution

```bash
bash -x script.sh
```

Bash prints a trace of commands as they are executed.

Use this for troubleshooting, but be careful: traces can reveal passwords, tokens, and other sensitive values.

## 38.3 Add tracing inside a script

```bash
set -x
```

To turn it off:

```bash
set +x
```

## 38.4 Install ShellCheck on Ubuntu

```bash
sudo apt update
sudo apt install shellcheck
```

Check a script:

```bash
shellcheck script.sh
```

ShellCheck identifies many common Bash and shell scripting mistakes and suggests improvements.

You should still understand the suggestions and test your script.

## 38.5 Recommended validation workflow

For each script:

```bash
bash -n script.sh
shellcheck script.sh
bash script.sh
```

If the script is designed to be executed directly, also test:

```bash
chmod +x script.sh
./script.sh
```

Test both successful cases and expected failure cases.

---

# 39. Security Best Practices

Shell scripts can modify files, permissions, services, and system configuration. A mistake can affect an entire server.

Follow these rules:

1. Quote variable expansions where appropriate.
2. Validate user input and command-line arguments.
3. Avoid running every script with `sudo`.
4. Never store passwords or cloud credentials directly in a script.
5. Do not execute downloaded scripts without reviewing and verifying them.
6. Avoid unsafe use of `eval`.
7. Use `--` with supported commands when passing paths that might begin with a hyphen.
8. Use temporary directories created with `mktemp`.
9. Avoid destructive commands such as `rm -rf` unless paths are carefully validated.
10. Test with sample data before using production data.
11. Set appropriate permissions on scripts, reports, and logs.
12. Handle command failures explicitly.
13. Avoid exposing secrets in logs or debug traces.
14. Follow the principle of least privilege.
15. Back up important files before making changes.
16. Verify the target host before running remote or destructive commands.
17. Use secure SSH authentication and protect private keys.
18. Test restoration of backups, not just backup creation.
19. Review scripts before scheduling them automatically.
20. Keep scripts under version control using Git.

### Example: Restrict access to a script

```bash
chmod 750 script.sh
```

This normally gives the owner read/write/execute permissions, the group read/execute permissions, and no permissions to others.

The correct permissions depend on who needs to run or modify the script.

---

# 40. Hands-On Practice Labs

Practice on your Ubuntu machine or a non-production EC2 instance.

Do not run destructive experiments on a production server.

## Lab 1: Basic commands

Tasks:

- [ ] Display the current user.
- [ ] Display the hostname.
- [ ] Display the date.
- [ ] Display the current directory.
- [ ] List files.
- [ ] Create a practice directory.
- [ ] Save command output into a text file.

Commands to explore:

```bash
whoami
hostname
date
pwd
ls -lah
mkdir -p ~/lab1
date > ~/lab1/report.txt
cat ~/lab1/report.txt
```

## Lab 2: Variables and arguments

Tasks:

- [ ] Create a variable containing your name.
- [ ] Print the variable.
- [ ] Create a script accepting a name as an argument.
- [ ] Print a useful usage message when the argument is missing.
- [ ] Return a non-zero status for invalid usage.

## Lab 3: Conditions and loops

Tasks:

- [ ] Check whether a directory exists.
- [ ] Check whether a file is empty.
- [ ] Display a warning for a number greater than a threshold.
- [ ] Print numbers from 1 to 10.
- [ ] Loop through `.log` files in a test directory.

## Lab 4: Backup automation

Tasks:

- [ ] Create a directory containing sample files.
- [ ] Write a backup script.
- [ ] Add a timestamp to the archive name.
- [ ] Verify that the archive exists.
- [ ] List archive contents.
- [ ] Extract the archive into a separate test directory.
- [ ] Compare the restored files with the originals.

## Lab 5: Disk and memory reporting

Tasks:

- [ ] Capture `df -h` output.
- [ ] Capture `free -h` output.
- [ ] Save the results to a timestamped report.
- [ ] Add a disk warning threshold.
- [ ] Test the threshold with a deliberately low value.

## Lab 6: Log analysis

Tasks:

- [ ] Create a sample log file.
- [ ] Count error lines.
- [ ] Display line numbers for errors.
- [ ] Handle a missing log file.
- [ ] Handle a log with no errors.
- [ ] Save a report.

## Lab 7: Service status

Tasks:

- [ ] Check an existing systemd service.
- [ ] Distinguish active status from enabled status.
- [ ] Read service logs using `journalctl`.
- [ ] Test an invalid service name.
- [ ] Do not stop or restart production services for practice.

## Lab 8: AWS EC2 practice

If you have an Ubuntu EC2 instance:

1. Connect using SSH.
2. Confirm the hostname and operating system.
3. Create a dedicated practice directory.
4. Upload or create your scripts.
5. Run syntax checks.
6. Run the disk usage script.
7. Run the server health checker.
8. Inspect generated reports.
9. Test a scheduled task on a safe schedule.
10. Remove only your own test files when finished.

Keep SSH private keys secure. Restrict inbound access to trusted sources where practical, and monitor AWS costs.

---

# 41. Common Errors and Troubleshooting

## Error 1: Permission denied

Example:

```text
bash: ./script.sh: Permission denied
```

Possible fix:

```bash
chmod +x script.sh
./script.sh
```

You can also run a readable script using:

```bash
bash script.sh
```

File permissions, directory permissions, mount options, and security controls can also cause execution failures.

## Error 2: Command not found

Example:

```text
command not found
```

Check spelling:

```bash
command -v bash
command -v shellcheck
```

The command may be missing or its location may not be in `PATH`.

## Error 3: No such file or directory

Check the current directory:

```bash
pwd
ls -lah
```

Use an appropriate relative or absolute path.

## Error 4: Bad interpreter

Example:

```text
bad interpreter
```

Check the shebang and whether the specified interpreter exists.

If a script was edited on Windows, it may have Windows-style line endings. On Ubuntu, you can convert it with:

```bash
dos2unix script.sh
```

Install `dos2unix` if needed, or use an editor that saves Unix line endings.

## Error 5: Unexpected end of file

Common causes include:

- Missing `fi`.
- Missing `done`.
- Missing `esac`.
- Unclosed quotes.
- Incomplete syntax.

Check:

```bash
bash -n script.sh
```

## Error 6: Variable is empty

Check variable spelling and assignment syntax.

Correct:

```bash
name="Linux"
echo "$name"
```

Incorrect:

```bash
name = "Linux"
```

Also remember that shell variables are generally local to their shell process unless exported or otherwise passed to another process.

## Error 7: Script works manually but not in cron

Check:

- Absolute script path.
- Script permissions.
- Cron user's permissions.
- Required environment variables.
- Absolute paths to required commands.
- Cron logs and redirected output.

## Error 8: Service not found

Check available service names:

```bash
systemctl list-unit-files --type=service
```

Ubuntu and RHEL-based distributions may use different names for similar services.

## Error 9: `grep` returns status 1

This normally means no matching lines were found. It does not always mean the log file could not be read.

Handle "no matches" and actual errors separately when they have different meanings in your application.

---

# 42. Shell Scripting Interview Questions and Answers

## Beginner-level questions

### 1. What is a shell?

A shell is a program that interprets commands and launches programs to perform requested operations.

### 2. What is Bash?

Bash stands for Bourne Again SHell. It is a commonly used command-line shell that also provides scripting features.

### 3. What is shell scripting?

Shell scripting is writing shell commands and logic in a file so that tasks can be executed together or automated.

### 4. What is the difference between a shell and a terminal?

A terminal provides an interface for text input and output. A shell interprets commands entered through that interface.

### 5. What is the difference between Bash and a shell script?

Bash is a shell program. A shell script is a file containing commands and scripting logic interpreted by a shell.

### 6. What is a shebang?

A shebang begins with `#!` and identifies the interpreter used when a script is executed directly.

### 7. Why do we use `chmod +x`?

It adds execute permission for the relevant permission classes, depending on the command's options and the current permissions.

### 8. What is the difference between `bash script.sh` and `./script.sh`?

`bash script.sh` explicitly starts Bash to read the script. `./script.sh` executes the file directly, normally using its shebang, and requires execute permission.

### 9. What is a variable?

A variable stores a value that can be referenced later in a script.

### 10. Why should we quote variables?

Quoting prevents many problems caused by word splitting and filename expansion, especially when values contain spaces or special characters.

## Intermediate-level questions

### 11. What is the difference between single and double quotes?

Single quotes preserve characters literally. Double quotes allow parameter expansion and command substitution while protecting most whitespace and special characters.

### 12. What does `$?` mean?

It contains the exit status of the most recently completed command or pipeline.

### 13. What is an exit code?

It is a status returned by a command or script. Conventionally, `0` indicates success and non-zero values indicate failure or another condition.

### 14. What is `$1`?

It refers to the first positional argument passed to a script or function.

### 15. What does `$#` represent?

It represents the number of positional arguments.

### 16. What is the difference between `"$@"` and `"$*"`?

Quoted `"$@"` expands each positional argument as a separate word. Quoted `"$*"` combines all positional arguments into one word separated by the first character of `IFS`.

### 17. What is the purpose of an if statement?

It allows a script to run different commands depending on whether a condition succeeds.

### 18. What is the purpose of a loop?

A loop repeats commands while iterating through values or while a condition holds.

### 19. What is a function?

A function groups reusable commands under a name.

### 20. What is command substitution?

It captures the output of a command using syntax such as `$(date)`.

### 21. What is a pipe?

A pipe, written `|`, passes the standard output of one command to the standard input of another.

### 22. What is redirection?

Redirection changes where a command reads input from or sends output and error messages to.

### 23. What is the difference between `>` and `>>`?

`>` overwrites or creates a file. `>>` appends to a file.

### 24. What is the difference between `grep` and `find`?

`grep` searches text for patterns. `find` searches filesystem paths based on criteria such as name, type, and size.

### 25. What is `awk` used for?

`awk` processes structured text, often by fields and patterns, and can calculate or format results.

## DevOps-focused questions

### 26. Why is shell scripting useful in DevOps?

It automates repetitive tasks such as deployment steps, server checks, backups, log analysis, and CI/CD commands.

### 27. How would you check disk usage using a script?

Use `df` to inspect filesystem usage and parse its output or use a suitable structured monitoring interface to compare usage against a threshold.

### 28. How would you check whether a service is running?

On a systemd-based Linux system, use:

```bash
systemctl is-active --quiet SERVICE_NAME
```

### 29. How would you find errors in a log file?

Use `grep`, for example:

```bash
grep -in "error" application.log
```

### 30. How would you create a compressed backup?

Use `tar` with gzip compression, such as:

```bash
tar -czf backup.tar.gz directory/
```

For production backups, also validate the destination, retention policy, archive integrity, and restoration process.

### 31. What is cron?

Cron is a scheduler used on many Linux systems to run commands at specified times.

### 32. How do you debug a Bash script?

Use syntax checking:

```bash
bash -n script.sh
```

and execution tracing:

```bash
bash -x script.sh
```

### 33. What is ShellCheck?

ShellCheck is a static analysis tool that identifies common shell scripting errors and suggests improvements.

### 34. What does `set -euo pipefail` do?

It enables several error-related Bash options: `-e` requests exit on many unhandled command failures, `-u` treats many unset variable expansions as errors, and `pipefail` changes pipeline status behavior. These options do not eliminate the need for explicit error handling.

### 35. What is an environment variable?

An environment variable is a name-value pair passed to child processes. It can hold configuration such as `PATH` or application settings.

### 36. Why should scripts validate user input?

Validation helps prevent unexpected behavior, accidental file operations, invalid arguments, and security problems.

### 37. Why should scripts use absolute paths in cron?

Cron often runs with a different working directory and a limited environment. Absolute paths reduce ambiguity.

### 38. How would you automate server health checks?

Create a script that collects hostname, uptime, filesystem usage, memory usage, and selected service status. Write results to a report and schedule or monitor the script appropriately.

### 39. Why is `eval` risky?

`eval` causes text to be interpreted again as shell code. If untrusted input reaches it, that input may execute unintended commands.

### 40. Is Bash suitable for every automation project?

No. Bash is excellent for command-line orchestration and straightforward system tasks. Python or another language may be easier to maintain for complex data processing, APIs, larger applications, and sophisticated error handling.

---

# 43. Quick Revision Cheat Sheet

## Essential commands

| Command | Use |
|---|---|
| `pwd` | Current directory |
| `ls -lah` | List files |
| `whoami` | Current username |
| `hostname` | System hostname |
| `date` | Date and time |
| `df -h` | Filesystem usage |
| `free -h` | Memory information |
| `uptime` | System uptime and load averages |
| `grep` | Search text |
| `find` | Search filesystem paths |
| `tar` | Archive files |
| `systemctl` | Inspect systemd services |
| `journalctl` | Read systemd journal logs |
| `crontab -e` | Edit the current user's cron schedule |
| `bash -n script.sh` | Check Bash syntax |
| `bash -x script.sh` | Trace script execution |
| `shellcheck script.sh` | Analyse shell script quality |

## Essential syntax

```bash
#!/usr/bin/env bash
```

Bash interpreter declaration.

```bash
name="Linux"
echo "$name"
```

Variable assignment and expansion.

```bash
read -r -p "Enter name: " name
```

Read input.

```bash
if [[ -f "$file" ]]; then
    echo "File exists"
fi
```

Condition.

```bash
for item in one two three; do
    echo "$item"
done
```

Loop.

```bash
my_function() {
    echo "Hello"
}
```

Function.

```bash
echo "$((10 + 5))"
```

Integer arithmetic.

```bash
result="$(date)"
```

Command substitution.

```bash
command > output.txt 2> error.txt
```

Separate output and error redirection.

```bash
command1 | command2
```

Pipe output between commands.

## Important reminders

- `0` conventionally means success; non-zero means failure or another condition.
- Use `"$variable"` for most variable expansions.
- Use `"$@"` when forwarding all arguments individually.
- Use `[[ ... ]]` for Bash conditions.
- Use `chmod +x script.sh` when you want to execute a script directly.
- Use `bash -n` to check syntax.
- Use ShellCheck to find common mistakes.
- Test scripts before using them on production servers.
- Never assume a backup is valid until you test restoring it.

---

# 44. Learning Checklist

Complete these topics in order.

## Foundation

- [ ] I understand what Linux is.
- [ ] I can explain what a shell is.
- [ ] I understand the difference between a terminal, shell, Bash, and script.
- [ ] I understand why DevOps engineers use shell scripting.
- [ ] I can check the shell and Bash version.

## Bash basics

- [ ] I can create and run a Bash script.
- [ ] I understand the shebang.
- [ ] I can use variables.
- [ ] I understand quoting.
- [ ] I can collect user input.
- [ ] I can accept command-line arguments.
- [ ] I understand exit statuses.

## Programming logic

- [ ] I can write `if`, `elif`, and `else` conditions.
- [ ] I can check files and directories.
- [ ] I can compare strings and integers.
- [ ] I can use `case`.
- [ ] I can write `for` and `while` loops.
- [ ] I can write functions.
- [ ] I can use arrays.
- [ ] I can perform integer arithmetic.

## Linux automation

- [ ] I can redirect output and errors.
- [ ] I can use pipes.
- [ ] I can search logs with `grep`.
- [ ] I can process text with `awk` and `sed`.
- [ ] I can read files line by line.
- [ ] I can check disk usage.
- [ ] I can check memory usage.
- [ ] I can check a systemd service.
- [ ] I can create and inspect a compressed backup.

## DevOps practice

- [ ] I have created a file backup script.
- [ ] I have created a disk usage checker.
- [ ] I have created a log analysis script.
- [ ] I have created a service status checker.
- [ ] I have completed the server health checker mini project.
- [ ] I can check script syntax.
- [ ] I have used ShellCheck.
- [ ] I understand basic cron scheduling.
- [ ] I have practised on Ubuntu or a safe EC2 test instance.
- [ ] I have tested script failure scenarios.
- [ ] I understand the security risks of running scripts with elevated permissions.

---

# Recommended Learning Order

Do not try to memorise every command in one day.

**Day 1:** Shell, terminal, Bash, and your first script.

**Day 2:** Variables, quoting, input, arguments, and exit codes.

**Day 3:** Conditions, file tests, and comparisons.

**Day 4:** Loops, functions, arrays, and arithmetic.

**Day 5:** Pipes, redirection, `grep`, `find`, `awk`, and `sed`.

**Day 6:** Build the backup, disk usage, log analysis, and service status scripts.

**Day 7:** Complete the server health checker, test failures, use ShellCheck, and review interview questions.

Continue practising the scripts on a safe Linux machine. Understanding why each command works is more valuable than memorising syntax.

---

# Official Learning Resources

- [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html) — the official Bash documentation.
- [ShellCheck](https://www.shellcheck.net/) — a shell script analysis tool.
- [Ubuntu Documentation](https://help.ubuntu.com/) — Ubuntu-specific help and documentation.

**Final advice:** Learn one concept, write a small script, run it, deliberately test a failure case, and explain the result in your own words. That is how you build practical shell scripting skills for Linux and DevOps.

