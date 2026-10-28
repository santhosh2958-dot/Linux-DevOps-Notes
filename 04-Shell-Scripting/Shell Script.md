## 1. What is a Shell Script?

A **shell script** is a text file that contains a series of **Linux/Unix commands**.
Instead of typing commands one by one in the terminal, you write them in a file and run it.

* **Manual work** → typing commands every time
* **Shell script** → automate that work with one command

Shell scripts are commonly used for:

* Automation (backups, cleanup)
* System administration
* File handling
* Monitoring system resources

---

## 2. What is a Shell?

A **shell** is a program that:

* Takes commands from the user
* Sends them to the operating system
* Shows the output

Common shells:

* `bash` (most popular)
* `sh`
* `zsh`
* `fish`

In most Linux systems, **Bash (Bourne Again Shell)** is the default.

---

## 3. Why `#!/bin/bash` at the Beginning?

This line is called a **shebang**.

```bash
#!/bin/bash
```

### Why it is needed:

* It tells the system **which shell should execute the script**
* Without it, the system may not know how to run the file

### How it works:

* `#!` → special characters (shebang)
* `/bin/bash` → path to the Bash shell

Meaning:

> “Run this script using Bash”

### Example:

If your script starts with:

```bash
#!/bin/bash
```

The OS knows:
Use **Bash** to interpret the commands inside the file.

---

## 4. Your First Shell Script (Beginner Level)

### Step 1: Create a file

```bash
nano hello.sh
```

### Step 2: Write this script

```bash
#!/bin/bash

echo "Hello, World!"
echo "Welcome to Shell Scripting"
```

### Step 3: Make it executable

```bash
chmod +x hello.sh
```

### Step 4: Run it

```bash
./hello.sh
```

---

## 5. Explanation of the Script

```bash
#!/bin/bash
```

Uses Bash shell

```bash
echo "Hello, World!"
```

`echo` prints text on the screen

```bash
echo "Welcome to Shell Scripting"
```

Prints another message

---

## 6. Shell Script with Variables & Input

```bash
#!/bin/bash

echo "Enter your name:"
read name

echo "Hello $name, welcome to Shell Scripting!"
```

### Explanation:

* `read name` → takes input from user
* `$name` → accesses the variable
* Variables do NOT need a data type

---

## 7. Beginner Shell Script with Conditions

```bash
#!/bin/bash

echo "Enter a number:"
read num

if [ $num -gt 10 ]
then
    echo "Number is greater than 10"
else
    echo "Number is 10 or less"
fi
```

### Explanation:

* `if`, `then`, `else`, `fi` → conditional statements
* `-gt` → greater than
* `[ ]` → test condition

---

## 8. Simple File Management System

**File Organizer Script**

### Project Goal:

* Create folders
* Move files based on extension
* Automate file organization

---

### Project Script

```bash
#!/bin/bash

# Create directories
mkdir -p Images Documents Scripts

# Move files based on extension
mv *.jpg *.png Images 2>/dev/null
mv *.pdf *.txt Documents 2>/dev/null
mv *.sh Scripts 2>/dev/null

echo "Files organized successfully!"
```

---

### Explanation

```bash
mkdir -p Images Documents Scripts
```

Creates folders (no error if already exists)

```bash
mv *.jpg *.png Images
```

Moves image files into `Images` folder

```bash
2>/dev/null
```

Hides error messages if no files found

```bash
echo "Files organized successfully!"
```

Prints success message

### UserManagment script

```bash
#!/bin/bash


# -------- Variables --------
USERNAME=$1
ACTION=$2

# -------- Function: show usage --------
usage() {
    echo "Usage: $0 <username> <action>"
    echo "Actions:"
    echo "  check  - Check if user exists"
    echo "  info   - Show user ID info"
}

# -------- Function: check user --------
check_user() {
    if id "$USERNAME" &>/dev/null
    then
        echo "User '$USERNAME' exists"
    else
        echo "User '$USERNAME' does NOT exist"
    fi
}

# -------- Function: user info --------
user_info() {
    if id "$USERNAME" &>/dev/null
    then
        echo "User information:"
        id "$USERNAME"
    else
        echo "Cannot show info. User does not exist"
    fi
}

# -------- Argument validation --------
if [ $# -ne 2 ]
then
    usage
    exit 1
fi

# -------- Action handler --------
if [ "$ACTION" == "check" ]
then
    check_user
elif [ "$ACTION" == "info" ]
then
    user_info
else
    echo "Invalid action"
    usage
fi
```
