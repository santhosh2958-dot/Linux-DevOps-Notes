# Linux Commands: `mkdir` — Create Directories

## 1. What is `mkdir`?

`mkdir` stands for **Make Directory**. It is a Linux command used to create new directories (folders).

**Example use cases:**
- Create project folders.
- Organize Linux and DevOps learning files.
- Create nested directory structures.
- Set permissions when creating directories.

## 2. Syntax

```bash
mkdir [OPTIONS] DIRECTORY_NAME
```

| Option | Meaning | Memory Trick |
|---|---|---|
| `-p` | Create parent directories as needed; do not fail if the target directory already exists | **p = Parents** |
| `-v` | Display a message for each directory created | **v = Verbose** |
| `-m MODE` | Set permissions for newly created directories | **m = Mode** |

## 3. Basic Examples

### Example 1: Create one directory

```bash
mkdir devops
```

Creates a directory named `devops` in the current location.

### Example 2: Create multiple directories

```bash
mkdir aws docker kubernetes
```

Creates three directories in the current location.

### Example 3: Create nested directories

```bash
mkdir -p projects/linux/scripts
```

Creates the directory structure:

```text
projects/
└── linux/
    └── scripts/
```

The `-p` option creates missing parent directories automatically.

### Example 4: Verbose mode

```bash
mkdir -v terraform
```

Possible output:

```text
mkdir: created directory 'terraform'
```

The `-v` option displays what was created.

### Example 5: Set directory permissions

```bash
mkdir -m 700 private
ls -ld private
```

Possible output:

```text
drwx------ ... private
```

Permission `700` means:

- Owner: read, write, and execute (`rwx`)
- Group: no permissions (`---`)
- Others: no permissions (`---`)

For a directory, execute permission allows users to access or traverse it, subject to other permissions.

## 4. Combining Options: `mkdir -pvm 700`

Your EC2 practice command was:

```bash
mkdir -pvm 700 script
```

Meaning:

- `-p`: create parent directories if needed and do not fail if the target already exists.
- `-v`: display a message when a directory is created.
- `-m 700`: set permissions to `700` on newly created directories.
- `script`: the directory name.

**Important:** You ran the same command twice. The output showed `created directory 'script'` both times, which is unexpected for the same existing directory in the same location. With `-p`, a second run should normally succeed silently if `script` already exists. Verify the actual directory and permissions with:

```bash
pwd
ls -ld script
stat -c '%A %a %n' script
```

Expected permissions for a directory created with mode `700` are `drwx------` and numeric mode `700`.

If the directory already exists, `mkdir -m 700 script` does **not** change its existing permissions. To change them, use:

```bash
chmod 700 script
```

## 5. Your AWS EC2 Hands-on Practice

Your current terminal shows:

```text
ubuntu@ip-172-31-2-84:~$
```

You ran:

```bash
mkdir -pvm 700 script
ls
pwd
ls -ld
```

Your results showed:

```text
script
/home/ubuntu
drwxr-x--- 5 ubuntu ubuntu 4096 Oct 28 14:49 .
```

Explanation:

- `ls` showed `script`, confirming that the directory is present.
- `pwd` showed `/home/ubuntu`, your current working directory.
- `ls -ld` displayed information about the current directory (`.`), **not** the `script` directory.
- `drwxr-x---` means the current `/home/ubuntu` directory has permissions `750`; it does not tell us the permissions of `script`.

To inspect the `script` directory specifically, run:

```bash
ls -ld ~/script
```

For a more detailed permission check:

```bash
stat -c 'Name: %n | Permissions: %a | Owner: %U | Group: %G' ~/script
```

## 6. Useful Differences

| Command | Purpose |
|---|---|
| `mkdir test` | Create a directory |
| `mkdir -p a/b/c` | Create nested directories |
| `mkdir -v test` | Print a creation message |
| `mkdir -m 700 private` | Create a directory with permissions `700` |
| `ls -l` | List directory contents |
| `ls -ld test` | Show details of the directory itself |
| `chmod 700 test` | Change permissions of an existing directory |
| `pwd` | Show the current working directory |
| `rmdir test` | Remove an empty directory |

## 7. Common Errors

**Error: `File exists`**

```text
mkdir: cannot create directory 'script': File exists
```

Reason: The directory already exists and `-p` was not used.

Check it:

```bash
ls -ld script
```

**Error: `Permission denied`**

Reason: You do not have permission to create the directory at the selected location.

Check your current location and permissions:

```bash
pwd
ls -ld .
```

## 8. Interview Questions

1. What is the purpose of the `mkdir` command?
2. What is the difference between `mkdir` and `mkdir -p`?
3. What does the `-v` option do?
4. What does `mkdir -m 700 private` mean?
5. What is the difference between `ls -l` and `ls -ld directory`?
6. Does `mkdir -m 700 existing_directory` change the existing directory's permissions?
7. How can you change the permissions of an existing directory?

## 9. Quick Revision

- `mkdir` → Create a directory.
- `-p` → Create missing parent directories.
- `-v` → Verbose output.
- `-m` → Set permissions at creation.
- `700` → Owner has `rwx`; group and others have no permissions.
- `ls -ld directory` → Inspect the directory itself.
- `chmod` → Change existing permissions.

## 10. Practice Checklist

- [ ] Create one directory.
- [ ] Create multiple directories.
- [ ] Create nested directories using `-p`.
- [ ] Use `-v` and observe the output.
- [ ] Create a directory with mode `700`.
- [ ] Inspect permissions using `ls -ld`.
- [ ] Change permissions using `chmod`.
- [ ] Explain the meaning of `drwx------`.

**Revision reminder:** Practise each command in your EC2 terminal and record the actual output. Understanding the output is just as important as memorizing the syntax.
