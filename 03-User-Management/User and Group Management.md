
# 03 — Linux User and Group Management

Complete beginner-friendly notes for Linux, Ubuntu, RHEL, Amazon Linux, and DevOps practice.

This page covers user management commands, groups, passwords, permissions, administrator access, practical examples, an EC2 lab, a mini project, troubleshooting, interview questions, and quick revision notes.

---

## Table of Contents

1. [Learning Objectives](#1-learning-objectives)
2. [Linux User Management Basics](#2-linux-user-management-basics)
3. [Important User and Group Concepts](#3-important-user-and-group-concepts)
4. [Important Linux Account Files](#4-important-linux-account-files)
5. [Check Current Users and Login Sessions](#5-check-current-users-and-login-sessions)
6. [User Information Commands](#6-user-information-commands)
7. [Create Users](#7-create-users)
8. [Set and Manage Passwords](#8-set-and-manage-passwords)
9. [Modify Users](#9-modify-users)
10. [Delete or Disable Users](#10-delete-or-disable-users)
11. [Linux Group Management](#11-linux-group-management)
12. [Switch Users and Use sudo](#12-switch-users-and-use-sudo)
13. [File Ownership and User Permissions](#13-file-ownership-and-user-permissions)
14. [Login Shells and Service Accounts](#14-login-shells-and-service-accounts)
15. [Password Aging and Account Expiry](#15-password-aging-and-account-expiry)
16. [Ubuntu vs RHEL vs Amazon Linux](#16-ubuntu-vs-rhel-vs-amazon-linux)
17. [Useful Advanced Commands](#17-useful-advanced-commands)
18. [Common Errors and Troubleshooting](#18-common-errors-and-troubleshooting)
19. [EC2 Hands-On Lab](#19-ec2-hands-on-lab)
20. [Mini Project: DevOps Team Access Management](#20-mini-project-devops-team-access-management)
21. [Real-World DevOps Use Cases](#21-real-world-devops-use-cases)
22. [35 Interview Questions and Answers](#22-35-interview-questions-and-answers)
23. [Command Cheat Sheet](#23-command-cheat-sheet)
24. [Revision Checklist](#24-revision-checklist)

---

# 1. Learning Objectives

After completing this topic, you should be able to:

- Understand Linux users, groups, UID, and GID.
- Explain root and administrator access.
- Create and manage Linux users.
- Set and change passwords safely.
- Create groups and add users to groups.
- Modify usernames, home directories, shells, and groups.
- Lock, unlock, and remove accounts safely.
- Understand important files such as `/etc/passwd` and `/etc/shadow`.
- Use `sudo` to run authorised administrative commands.
- Manage file ownership using `chown` and `chgrp`.
- Troubleshoot common user-management errors.
- Perform hands-on practice on an Ubuntu EC2 instance.
- Complete a mini project similar to a real DevOps task.

---

# 2. Linux User Management Basics

Linux is a multi-user operating system. Multiple users can use the same server while having different permissions.

For example, a DevOps server might have:

- `ubuntu` — default login account on many Ubuntu EC2 images.
- `ec2-user` — default login account on many Amazon Linux images.
- `developer1` — developer account.
- `tester1` — testing account.
- `root` — superuser account.
- `jenkins` — possible service account for a Jenkins installation.

The actual accounts depend on the installed operating system and applications.

## Why do we need user management?

Imagine five developers working on the same Linux server.

Without proper user management:
- Everyone might share one account.
- It would be difficult to identify who performed an action.
- Files could be modified or deleted by the wrong person.
- Every developer might have unnecessary administrator access.

With proper user management:
- Each person has an individual account.
- Permissions can be granted through groups.
- Access can be restricted.
- Account access can be removed when someone leaves the team.

**DevOps example:** An administrator creates accounts for developers, adds them to an approved group, and grants access only to the project directory they need.

---

# 3. Important User and Group Concepts

## 3.1 User

A user is an account identity used to access Linux and own files.

Example:

```bash
whoami
```

Possible output:

```text
ubuntu
```

This means the current effective user is `ubuntu`.

## 3.2 UID — User ID

UID means User ID.

Linux internally identifies users using numeric IDs.

Example:

```bash
id ubuntu
```

Possible output:

```text
uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),27(sudo)
```

Here:
- `uid=1000` — user's numeric ID.
- `gid=1000` — primary group's numeric ID.
- `groups=...` — groups associated with the user.

UID ranges vary by distribution and configuration. Do not assume every normal user starts at exactly 1000.

## 3.3 GID — Group ID

GID means Group ID.

Linux uses a numeric ID to identify each group.

Example:

```bash
getent group sudo
```

Possible output:

```text
sudo:x:27:
```

Here, `sudo` is the group name and `27` is its GID.

## 3.4 Primary group

Every normal user has a primary group.

It is commonly used as the default group for files the user creates.

Check it with:

```bash
id username
```

## 3.5 Supplementary groups

A user can belong to additional groups.

Example:

```bash
sudo usermod -aG developers username
```

This adds the user to the supplementary group `developers`.

## 3.6 Root user

The root account is the Linux superuser.

Its UID is normally `0`.

Root can perform most administrative operations, including:
- Installing packages.
- Managing accounts.
- Changing file ownership.
- Modifying system configuration.
- Restarting services.

Use root privileges carefully.

## 3.7 sudo

`sudo` allows an authorised user to run a command with elevated privileges.

Example:

```bash
sudo whoami
```

Possible output:

```text
root
```

This does not mean your normal login session permanently changed to root. Only that command ran with elevated privileges.

## 3.8 Home directory

A home directory stores a user's personal files and configuration.

Common examples:

```text
/home/ubuntu
/home/developer1
/root
```

The root account normally uses `/root`, not `/home/root`.

## 3.9 Login shell

A shell provides a command-line interface.

Common shells include:

```text
/bin/bash
/bin/sh
```

Check the current shell:

```bash
echo "$SHELL"
```

This shows the shell environment variable and may not always identify the shell currently executing your command.

---

# 4. Important Linux Account Files

Linux stores local account and group information in several files.

## 4.1 `/etc/passwd`

Contains basic account information.

View it:

```bash
cat /etc/passwd
```

Find a particular user:

```bash
getent passwd ubuntu
```

Example format:

```text
ubuntu:x:1000:1000:Ubuntu User:/home/ubuntu:/bin/bash
```

The fields are:

| Field | Meaning |
|---|---|
| `ubuntu` | Username |
| `x` | Password information is stored elsewhere |
| `1000` | UID |
| `1000` | Primary GID |
| `Ubuntu User` | Comment or descriptive information |
| `/home/ubuntu` | Home directory |
| `/bin/bash` | Login shell |

Do not assume every account uses `/bin/bash`; service accounts often use a non-interactive shell.

## 4.2 `/etc/shadow`

Stores protected password-hash and password-aging information for local accounts.

Do not share its contents.

Check account status using:

```bash
sudo passwd -S username
sudo chage -l username
```

These commands are safer for routine inspection than displaying password data.

## 4.3 `/etc/group`

Contains local group information.

View it:

```bash
cat /etc/group
```

Search for a group:

```bash
getent group developers
```

Example:

```text
developers:x:1002:alice,bob
```

The fields are:
- Group name.
- Password placeholder.
- GID.
- Listed supplementary group members.

A user's primary group membership may not appear in the final member list. Use `id username` to inspect actual memberships.

## 4.4 `/etc/gshadow`

Contains protected local group security information.

It should not be shared or edited casually.

## 4.5 `/etc/skel`

Contains default files that may be copied into new users' home directories.

View it:

```bash
ls -la /etc/skel
```

## 4.6 `/etc/sudoers`

Defines sudo policy.

Inspect allowed commands with:

```bash
sudo -l
```

If you must edit sudo policy, use:

```bash
sudo visudo
```

Never edit `/etc/sudoers` with a normal text editor. A syntax error can break administrator access.

**Important:** These files describe local accounts. Systems using LDAP, Active Directory, or another identity provider may also retrieve accounts from external sources. `getent` is often more useful than reading a local file alone.

---

# 5. Check Current Users and Login Sessions

## 5.1 `whoami`

Displays the effective username.

```bash
whoami
```

## 5.2 `id`

Displays UID, primary GID, and group memberships.

```bash
id
id ubuntu
```

## 5.3 `who`

Shows logged-in sessions.

```bash
who
```

## 5.4 `w`

Shows logged-in users and session activity.

```bash
w
```

## 5.5 `users`

Displays usernames associated with current login sessions.

```bash
users
```

## 5.6 `last`

Displays recorded login history when the system has the relevant accounting data.

```bash
last
```

Use this command in accordance with your organisation's privacy and audit policies.

## 5.7 `lastlog`

Shows last-login information on systems that provide the command and its underlying data.

```bash
lastlog
```

It may be missing or behave differently on some modern distributions.

## 5.8 Check whether a username exists

```bash
id devuser
getent passwd devuser
```

If `id` reports that the user does not exist, verify the spelling and account source.

---

# 6. User Information Commands

| Command | Purpose |
|---|---|
| `whoami` | Show effective username |
| `id` | Show user and group IDs |
| `who` | Show logged-in sessions |
| `w` | Show session activity |
| `users` | Show logged-in usernames |
| `last` | Review recorded login history |
| `lastlog` | Review last-login information where supported |
| `getent passwd` | Look up account entries |
| `getent group` | Look up group entries |
| `groups username` | Show group memberships |
| `finger username` | Additional account details if the optional tool is installed |

Examples:

```bash
whoami
id
groups
groups ubuntu
getent passwd ubuntu
getent group sudo
```

**Memory trick:** `whoami` tells you who you are; `id` tells you your numeric identity and groups.

---

# 7. Create Users

There are two common commands: `useradd` and `adduser`.

## 7.1 `useradd`

Creates a user account.

Basic syntax:

```bash
sudo useradd username
```

For a normal test account, a home directory and login shell are often useful:

```bash
sudo useradd -m -s /bin/bash devuser
```

Options:

| Option | Meaning |
|---|---|
| `-m` | Create the home directory |
| `-s` | Set the login shell |
| `-d` | Specify the home directory |
| `-c` | Set descriptive comment information |
| `-u` | Specify a UID |
| `-g` | Set the primary group |
| `-G` | Set supplementary groups at account creation |
| `-r` | Create a system account on supported implementations |

Set a password interactively:

```bash
sudo passwd devuser
```

Verify the account:

```bash
id devuser
getent passwd devuser
ls -ld /home/devuser
```

### Create a user with a descriptive comment

```bash
sudo useradd -m -s /bin/bash -c "DevOps Test User" devuser
```

### Create a user with a specified home directory

```bash
sudo useradd -m -d /home/devuser devuser
```

The command may fail if the account or home directory already exists. Check first rather than blindly repeating it.

## 7.2 `adduser`

On Ubuntu, `adduser` is a friendly interactive helper.

```bash
sudo adduser devuser
```

It normally guides you through account creation and password setup.

Availability and behaviour differ across Linux distributions. On RHEL-family systems, `useradd` is the standard portable choice.

## 7.3 Create a user with a specific primary group

The group must already exist.

```bash
sudo groupadd developers
sudo useradd -m -g developers devuser
```

Verify:

```bash
id devuser
```

## 7.4 Create a user with supplementary groups

```bash
sudo useradd -m -G developers,testing devuser
```

The groups must already exist.

## 7.5 Check the user's home directory

```bash
ls -ld /home/devuser
ls -la /home/devuser
```

`ls -ld` displays information about the directory itself, rather than listing only its contents.

---

# 8. Set and Manage Passwords

## 8.1 Set a user's password

```bash
sudo passwd devuser
```

The command prompts you securely for the password.

## 8.2 Change your own password

```bash
passwd
```

## 8.3 Check password status

```bash
sudo passwd -S devuser
```

Output formats vary by distribution.

## 8.4 Lock password authentication

```bash
sudo passwd -l devuser
```

This locks password authentication for the account according to the system's password mechanism.

**Important:** It does not necessarily block SSH public-key authentication or every other login method.

## 8.5 Unlock password authentication

```bash
sudo passwd -u devuser
```

Use this only when the account should be unlocked and the operation is permitted by the system.

## 8.6 Check password aging

```bash
sudo chage -l devuser
```

This displays password-aging information.

### Password security rules

- Do not put passwords directly in shell commands or scripts.
- Do not reuse personal passwords for test accounts.
- Do not share password hashes from `/etc/shadow`.
- Follow your organisation's password and authentication policy.
- For EC2, remember that password authentication for SSH may be disabled by default.

---

# 9. Modify Users

The main command is `usermod`.

General syntax:

```bash
sudo usermod [options] username
```

## 9.1 Change the user's descriptive information

```bash
sudo usermod -c "DevOps Engineer" devuser
```

Verify:

```bash
getent passwd devuser
```

## 9.2 Change the login shell

```bash
sudo usermod -s /bin/bash devuser
```

## 9.3 Add a user to a supplementary group

```bash
sudo usermod -aG developers devuser
```

**Very important:** Use `-aG` when adding supplementary groups. The `-a` means append. Omitting it while using `-G` can replace the user's existing supplementary group list.

## 9.4 Change the user's home directory

```bash
sudo usermod -d /home/devuser-new -m devuser
```

- `-d` sets the new home directory.
- `-m` moves the contents to the new location.

Check that the destination is suitable and that the operation will not overwrite or mix unrelated data.

## 9.5 Lock password authentication

```bash
sudo usermod -L devuser
```

## 9.6 Unlock password authentication

```bash
sudo usermod -U devuser
```

The `passwd -l` and `passwd -u` commands are also available on many systems.

## 9.7 Verify changes

```bash
id devuser
getent passwd devuser
groups devuser
```

**Remember:** After changing group membership, an existing login session may still have the old group list. Log out and back in before testing access.

---

# 10. Delete or Disable Users

Removing a user can affect files, services, scheduled jobs, and audit history. Check dependencies first.

## 10.1 Delete a user but leave their home directory

```bash
sudo userdel devuser
```

The home directory and other files may remain.

## 10.2 Delete a user and attempt to remove the home directory

```bash
sudo userdel -r devuser
```

This also attempts to remove the home directory and mail spool.

**Warning:** This can permanently delete user data. Confirm the account, backups, and exact target before running it.

## 10.3 Check the user before deletion

```bash
id devuser
getent passwd devuser
sudo find /home -maxdepth 2 -user devuser -ls
```

The `find` command searches only the specified path. Other user-owned files may exist elsewhere.

## 10.4 Safer account offboarding process

1. Confirm that the account is no longer needed.
2. Check file ownership and application dependencies.
3. Preserve required files and audit records.
4. Disable access according to organisational policy.
5. Check scheduled jobs, services, SSH keys, and running processes.
6. Obtain approval for permanent deletion.
7. Remove the account only when safe.

Do not delete your current EC2 login account or remove your only administrator access while practising.

---

# 11. Linux Group Management

Groups simplify permission management.

For example, instead of giving access to ten developers individually, create one group named `developers` and grant that group access to the project directory.

## 11.1 Create a group

```bash
sudo groupadd developers
```

Verify:

```bash
getent group developers
```

## 11.2 Add a user to a group

```bash
sudo usermod -aG developers alice
```

Another option on many distributions:

```bash
sudo gpasswd -a alice developers
```

## 11.3 Check group membership

```bash
id alice
groups alice
getent group developers
```

## 11.4 Remove a user from a group

```bash
sudo gpasswd -d alice developers
```

Alternatively, use `usermod -G` only when you know the complete supplementary group list you want to set. It replaces that list rather than appending to it.

## 11.5 Rename a group

```bash
sudo groupmod -n engineering developers
```

Verify:

```bash
getent group engineering
```

## 11.6 Change a group's GID

```bash
sudo groupmod -g 2005 engineering
```

Changing a GID can affect files that still carry the old numeric group ID. Plan the change carefully.

## 11.7 Delete a group

```bash
sudo groupdel engineering
```

A group cannot normally be deleted while it is a user's primary group. Check usage before deleting.

## 11.8 Understand primary and supplementary groups

Check a user:

```bash
id alice
```

Example:

```text
uid=1001(alice) gid=1001(alice) groups=1001(alice),1002(developers)
```

In this example:
- `alice` is the user.
- The group with GID `1001` is the primary group.
- `developers` is a supplementary group.

## 11.9 Refresh group membership

After adding a user to a group, start a new login session and verify:

```bash
id
```

The `newgrp` command can start a shell with a selected group as the effective group in suitable circumstances:

```bash
newgrp developers
```

It does not replace understanding how login sessions and group memberships work.

---

# 12. Switch Users and Use sudo

## 12.1 `su`

Switch to another user:

```bash
su - devuser
```

The hyphen requests a login-style environment.

Return to the previous shell:

```bash
exit
```

## 12.2 Switch to root

```bash
su -
```

This works only if the root account and authentication policy permit it.

## 12.3 Use sudo for one command

```bash
sudo whoami
sudo id
sudo ls /root
```

The third command is an example of reading a restricted location with approved privileges. Use elevated access only when needed.

## 12.4 Check sudo permissions

```bash
sudo -l
```

This shows commands the current user is allowed to run through sudo.

## 12.5 Ubuntu administrator group

Ubuntu commonly uses the `sudo` group.

For an approved administrator account:

```bash
sudo usermod -aG sudo devuser
```

## 12.6 RHEL and Amazon Linux administrator group

RHEL-family systems and Amazon Linux commonly use the `wheel` group.

For an approved administrator account:

```bash
sudo usermod -aG wheel devuser
```

Actual policy can vary. Verify the system configuration before making changes.

**Security rule:** Do not give every developer administrator access by default. Use least privilege.

---

# 13. File Ownership and User Permissions

Every file has an owner and a group.

Example:

```bash
ls -l
```

Possible output:

```text
-rw-r--r-- 1 alice developers 120 Oct 28 10:00 notes.txt
```

This indicates:
- Owner: `alice`
- Group: `developers`
- Permissions: owner can read/write; group and others can read.

## 13.1 Change file owner

```bash
sudo chown alice notes.txt
```

## 13.2 Change owner and group

```bash
sudo chown alice:developers notes.txt
```

## 13.3 Change group ownership

```bash
sudo chgrp developers notes.txt
```

## 13.4 Change ownership recursively

```bash
sudo chown -R alice:developers /opt/project-lab
```

**Warning:** `-R` changes ownership recursively. Use it only on the intended lab or application directory.

## 13.5 Understand permission values

| Number | Permission |
|---|---|
| `7` | Read + write + execute |
| `6` | Read + write |
| `5` | Read + execute |
| `4` | Read only |
| `0` | No permissions |

For example:

```bash
chmod 750 project.sh
```

This means:
- Owner: `7` — read, write, execute.
- Group: `5` — read, execute.
- Others: `0` — no access.

## 13.6 Shared group directory

A team directory can use group ownership and the setgid bit:

```bash
sudo mkdir -p /opt/team-lab
sudo chown root:developers /opt/team-lab
sudo chmod 2770 /opt/team-lab
```

The `2` sets the setgid bit. New entries normally inherit the directory's group. The `770` permissions give owner and group full directory access and no access to others.

Verify:

```bash
ls -ld /opt/team-lab
```

Expected permission shape:

```text
drwxrws---
```

The exact owner and group names depend on your configuration.

---

# 14. Login Shells and Service Accounts

## 14.1 View available shells

```bash
cat /etc/shells
```

Check whether a command exists:

```bash
command -v bash
command -v nologin
```

A `nologin` executable is commonly used to prevent normal interactive logins for service accounts. Its path varies by distribution.

## 14.2 Change a user's shell

```bash
sudo usermod -s /bin/bash devuser
```

## 14.3 Create a service account

Example on systems supporting these options:

```bash
sudo useradd --system --no-create-home --shell /usr/sbin/nologin appsvc
```

Check the `nologin` path first; some systems use a different location.

A service account is used by an application or background service rather than a human user.

Examples may include:
- Application services.
- Monitoring agents.
- Web server processes.
- CI/CD services.

**Important:** Service-account requirements differ. Some services require a home directory or writable application directories. Follow the application's installation documentation and grant only the permissions it needs.

---

# 15. Password Aging and Account Expiry

The `chage` command manages password-aging settings for supported local accounts.

## 15.1 Display password-aging information

```bash
sudo chage -l devuser
```

## 15.2 Set the maximum password age

```bash
sudo chage -M 90 devuser
```

This sets a maximum password age of 90 days.

## 15.3 Set the minimum password age

```bash
sudo chage -m 1 devuser
```

This sets a minimum age of one day between password changes.

## 15.4 Set the warning period

```bash
sudo chage -W 7 devuser
```

This configures a warning period of seven days before password expiry.

## 15.5 Set an account expiry date

Example:

```bash
sudo chage -E 2026-12-31 devuser
```

Use a date appropriate for your lab and policy. This example is a calendar date, not a universal recommendation.

Password expiry and account expiry are different concepts. An account-expiry date can prevent the account from being used after the configured date.

Follow your organisation's security policy rather than applying arbitrary password-aging settings to production accounts.

---

# 16. Ubuntu vs RHEL vs Amazon Linux

| Feature | Ubuntu | RHEL | Amazon Linux |
|---|---|---|---|
| Common initial EC2 login | `ubuntu` | Image-dependent | `ec2-user` |
| Common admin group | `sudo` | `wheel` | `wheel` |
| Friendly account helper | `adduser` commonly available | Behaviour may differ | Behaviour may differ |
| Portable account command | `useradd` | `useradd` | `useradd` |
| Change password | `passwd` | `passwd` | `passwd` |
| Modify account | `usermod` | `usermod` | `usermod` |
| Group management | `groupadd`, `groupmod`, `groupdel` | Same common tools | Same common tools |

Always verify the actual operating system, default username, installed commands, and administrator policy.

Check the operating system:

```bash
cat /etc/os-release
```

Check your identity:

```bash
whoami
id
```

---

# 17. Useful Advanced Commands

## 17.1 Find files owned by a user

```bash
sudo find /home -user alice -ls
```

This searches `/home`; it does not search every filesystem.

## 17.2 Find files owned by a group

```bash
sudo find /opt -group developers -ls
```

## 17.3 Inspect file metadata

```bash
stat notes.txt
```

This shows file metadata such as owner, group, size, and permission information.

## 17.4 Inspect a directory's ownership

```bash
ls -ld /opt/team-lab
```

## 17.5 Display numeric ownership

```bash
ls -ln notes.txt
```

This displays numeric UID and GID values rather than resolving them to names.

## 17.6 Check whether a user exists

```bash
getent passwd alice
```

## 17.7 Check whether a group exists

```bash
getent group developers
```

## 17.8 Check a user's group membership

```bash
id alice
groups alice
```

Use these verification commands after every account or group change.

---

# 18. Common Errors and Troubleshooting

## Error 1: User already exists

Example:

```text
useradd: user 'devuser' already exists
```

Check:

```bash
id devuser
getent passwd devuser
```

Use the existing account if appropriate instead of creating a duplicate.

## Error 2: Group already exists

Check:

```bash
getent group developers
```

## Error 3: Permission denied

Check:

```bash
whoami
id
ls -ld /path/to/directory
```

Confirm that the command is authorised and that the relevant directory permissions allow the operation.

## Error 4: Group does not exist

Example:

```text
usermod: group 'developers' does not exist
```

Check:

```bash
getent group developers
```

Create the group first if appropriate.

## Error 5: User does not have new group access

The current session may have old group information.

Check:

```bash
id username
```

Log out and back in, then test again.

## Error 6: User cannot run sudo

Check:

```bash
id username
sudo -l
```

Ubuntu commonly uses `sudo`; RHEL and Amazon Linux commonly use `wheel`. Ask the system administrator if the account is not authorised.

## Error 7: Cannot delete a group

The group may be a user's primary group.

Check:

```bash
getent group groupname
id username
```

Do not change a user's primary group merely to force deletion without understanding the consequences.

## Error 8: Cannot delete a user because processes are running

Check which processes belong to the account using approved administrative tools. Follow a safe service shutdown and offboarding procedure before removing the account.

## Error 9: Home directory is missing

Check:

```bash
getent passwd username
ls -ld /home/username
```

A missing home directory may result from an account being created without `-m`, a custom home path, or an earlier removal. Investigate before creating or changing directories.

## Troubleshooting rule

**Inspect first → identify the cause → make the smallest safe change → verify the result.**

---

# 19. EC2 Hands-On Lab

This lab is for a disposable test EC2 instance or a Linux VM. It creates local Linux accounts; it does not create AWS IAM users.

Do not use production accounts or change the only administrator account.

## Step 1: Identify your Linux environment

```bash
whoami
id
hostname
cat /etc/os-release
```

Record the operating system and the user you are logged in as.

## Step 2: Create a group

```bash
sudo groupadd devops-team
getent group devops-team
```

If the group already exists, inspect it rather than repeating the creation command.

## Step 3: Create two test users

```bash
sudo useradd -m -s /bin/bash alice
sudo useradd -m -s /bin/bash bob
```

Set passwords interactively if your lab requires password authentication:

```bash
sudo passwd alice
sudo passwd bob
```

On an EC2 server, do not enable password-based SSH access merely for this exercise. Follow your approved authentication policy.

## Step 4: Add the users to the team

```bash
sudo usermod -aG devops-team alice
sudo usermod -aG devops-team bob
```

Verify:

```bash
id alice
id bob
getent group devops-team
```

## Step 5: Create a shared directory

```bash
sudo mkdir -p /opt/devops-team
sudo chown root:devops-team /opt/devops-team
sudo chmod 2770 /opt/devops-team
```

Verify:

```bash
ls -ld /opt/devops-team
```

Expected permission shape:

```text
drwxrws---
```

## Step 6: Test file creation

```bash
sudo -u alice touch /opt/devops-team/alice-note.txt
sudo -u bob touch /opt/devops-team/bob-note.txt
```

List the files:

```bash
ls -l /opt/devops-team
```

If a test fails, check the users' group memberships and directory permissions before changing anything.

## Step 7: Record your results

```bash
{
  date
  whoami
  id alice
  id bob
  getent group devops-team
  ls -ld /opt/devops-team
  ls -l /opt/devops-team
} | tee ~/user-management-lab-report.txt
```

This saves a report in your current user's home directory.

## Step 8: Clean up the lab

First verify that `/opt/devops-team` contains only your lab files.

Then, when you no longer need the lab:

```bash
sudo rm -r /opt/devops-team
sudo userdel -r alice
sudo userdel -r bob
sudo groupdel devops-team
```

**Warning:** `rm -r` and `userdel -r` can permanently remove data. Verify the target paths and account names before running these commands. Never run the cleanup against real users or a production project directory.

---

# 20. Mini Project: DevOps Team Access Management

## Project objective

Create two Linux users who need access to the same project directory.

You will:
- Create a project group.
- Create two users.
- Add both users to the group.
- Create a shared directory.
- Configure ownership and permissions.
- Test access.
- Generate a verification report.

## Scenario

A DevOps team has two members:

- `tester1`
- `tester2`

Both members need to work in:

```text
/opt/project-team
```

They should be able to create and modify files in that directory. Other users should not receive access through the directory's ordinary permission bits.

## Step 1: Create the group

```bash
sudo groupadd project-team
```

## Step 2: Create the users

```bash
sudo useradd -m -s /bin/bash tester1
sudo useradd -m -s /bin/bash tester2
```

## Step 3: Add users to the group

```bash
sudo usermod -aG project-team tester1
sudo usermod -aG project-team tester2
```

## Step 4: Create the shared directory

```bash
sudo mkdir -p /opt/project-team
```

## Step 5: Set ownership

```bash
sudo chown root:project-team /opt/project-team
```

## Step 6: Set permissions

```bash
sudo chmod 2770 /opt/project-team
```

Explanation:
- `2` — setgid bit.
- `7` — owner has read, write, and execute permissions.
- `7` — group has read, write, and execute permissions.
- `0` — others have no permissions through these ordinary mode bits.

The setgid bit helps new entries inherit the directory's group. Actual file permissions can also be influenced by the creating process's umask and other security controls.

## Step 7: Test both users

```bash
sudo -u tester1 touch /opt/project-team/tester1.txt
sudo -u tester2 touch /opt/project-team/tester2.txt
```

Verify:

```bash
id tester1
id tester2
getent group project-team
ls -ld /opt/project-team
ls -l /opt/project-team
```

If a write fails, inspect the directory permissions and group membership. Do not immediately use `chmod 777`; it grants unnecessary access to others.

## Step 8: Generate a project report

```bash
{
  date
  id tester1
  id tester2
  getent group project-team
  ls -ld /opt/project-team
  ls -l /opt/project-team
} | tee ~/project-team-report.txt
```

## Expected learning outcomes

At the end of this project, you should understand:

1. Why teams use groups.
2. How UID and GID identify accounts.
3. Why `usermod -aG` is important.
4. How ownership and permissions work together.
5. Why `2770` can be useful for shared team directories.
6. How to verify configuration rather than assuming commands worked.

## Project checklist

- [ ] Created the `project-team` group.
- [ ] Created both test users.
- [ ] Added both users to the group.
- [ ] Created `/opt/project-team`.
- [ ] Set its group ownership correctly.
- [ ] Set permissions to `2770`.
- [ ] Tested file creation by both users.
- [ ] Saved the project report.
- [ ] Cleaned up only the test accounts and test files after checking the paths.

---

# 21. Real-World DevOps Use Cases

## Use case 1: Developer onboarding

A new developer joins the team.

Typical workflow:
1. Create an individual account.
2. Add the user to approved groups.
3. Grant access to the required project directory.
4. Verify permissions.
5. Document the change.

## Use case 2: Team directory

Multiple developers need a shared workspace.

Typical workflow:
1. Create a group.
2. Add approved users.
3. Create the shared directory.
4. Set group ownership.
5. Set appropriate permissions.
6. Test access as each user.

## Use case 3: Employee offboarding

A developer leaves the team.

Typical workflow:
1. Confirm the offboarding request.
2. Review the user's access and owned files.
3. Preserve required files and audit records.
4. Revoke access according to policy.
5. Check jobs, services, SSH access, and dependencies.
6. Remove the account when authorised.

## Use case 4: Service account

An application needs to run on a server.

Typical workflow:
1. Create a dedicated service account if required.
2. Avoid unnecessary interactive login.
3. Grant access only to the required directories and resources.
4. Follow the application's documented security requirements.
5. Monitor and review permissions.

## Use case 5: EC2 server access

A team needs to administer an EC2 Linux instance.

Remember:
- Linux users are not the same as AWS IAM users.
- EC2 SSH access depends on the instance's operating system, authentication configuration, network rules, and key or identity setup.
- Do not disable or delete your only working administrator account.
- Test account changes with a separate session and a verified recovery method.

---

# 22. 35 Interview Questions and Answers

## Beginner Questions

### Q1. What is Linux user management?

**Answer:** It is the process of creating, modifying, securing, and deleting user accounts on a Linux system.

### Q2. What is a Linux user?

**Answer:** A user is an account identity that can own files, run processes, and access permitted resources.

### Q3. What is UID?

**Answer:** UID means User ID. It is the numeric identity assigned to a user.

### Q4. What is GID?

**Answer:** GID means Group ID. It is the numeric identity assigned to a group.

### Q5. What is the root user?

**Answer:** Root is the superuser, normally UID `0`, with broad administrative privileges.

### Q6. What is a Linux group?

**Answer:** A group is a collection of users used to manage access and permissions more efficiently.

### Q7. What is a primary group?

**Answer:** It is the main group associated with a user and is commonly used as the default group for newly created files.

### Q8. What is a supplementary group?

**Answer:** It is an additional group that can provide a user with access to more resources.

### Q9. What is the difference between `useradd` and `adduser`?

**Answer:** `useradd` is a standard low-level account creation command. `adduser` is a friendlier helper commonly available on Ubuntu.

### Q10. How do you create a user?

**Answer:**

```bash
sudo useradd -m -s /bin/bash devuser
```

The `-m` option requests a home directory and `-s` selects the login shell.

## Commands and Account Files

### Q11. How do you set a user's password?

**Answer:**

```bash
sudo passwd devuser
```

It prompts for the password interactively.

### Q12. How do you check a user's UID and groups?

**Answer:**

```bash
id devuser
```

### Q13. How do you check whether a user exists?

**Answer:**

```bash
getent passwd devuser
```

### Q14. What is `/etc/passwd`?

**Answer:** It contains basic account information, including username, UID, primary GID, home directory, and login shell.

### Q15. What is `/etc/shadow`?

**Answer:** It stores protected password-hash and password-aging information for local accounts.

### Q16. What is `/etc/group`?

**Answer:** It contains local group information, including group names, GIDs, and listed supplementary members.

### Q17. What is `/etc/skel`?

**Answer:** It contains default files that may be copied into new users' home directories.

### Q18. What is the `usermod` command?

**Answer:** It modifies an existing user account, including group membership, shell, and home directory.

### Q19. How do you add a user to a group?

**Answer:**

```bash
sudo usermod -aG developers devuser
```

The `-a` appends the supplementary group rather than replacing the existing list.

### Q20. How do you remove a user from a group?

**Answer:** On many Linux systems:

```bash
sudo gpasswd -d devuser developers
```

### Q21. How do you create a group?

**Answer:**

```bash
sudo groupadd developers
```

### Q22. How do you change a user's login shell?

**Answer:**

```bash
sudo usermod -s /bin/bash devuser
```

### Q23. How do you delete a user?

**Answer:**

```bash
sudo userdel devuser
```

Check dependencies and preserve required data first.

### Q24. What does `userdel -r` do?

**Answer:** It removes the account and attempts to remove its home directory and mail spool. It can permanently delete data.

## Security and Permissions

### Q25. What is the difference between `su` and `sudo`?

**Answer:** `su` switches to another user or shell. `sudo` runs an authorised command with elevated privileges.

### Q26. How do you check sudo permissions?

**Answer:**

```bash
sudo -l
```

### Q27. Why should you use `visudo`?

**Answer:** It validates sudoers syntax and helps prevent configuration errors that could break sudo access.

### Q28. What does `passwd -l` do?

**Answer:** It locks password authentication for an account. Other authentication methods may still be possible.

### Q29. How do you check password-aging information?

**Answer:**

```bash
sudo chage -l devuser
```

### Q30. What does `chown` do?

**Answer:** It changes the owner of a file or directory and can also change its group.

### Q31. What does `chgrp` do?

**Answer:** It changes the group ownership of a file or directory.

### Q32. Why is `chmod 777` usually a poor solution?

**Answer:** It grants read, write, and execute permissions to everyone. This can expose data and allow unwanted modifications.

## Scenario-Based Questions

### Q33. A user cannot run sudo. What would you check?

**Answer:** I would check `id username`, `sudo -l`, the approved administrator group, and the system's sudo policy. I would not grant admin access without authorisation.

### Q34. You added a user to a group, but access still fails. Why?

**Answer:** The user's current session may have stale group membership. I would verify with `id username`, start a new login session, and check the directory permissions.

### Q35. How would you create a shared directory for a DevOps team?

**Answer:** I would create a group, add the approved users, create the directory, set its group ownership, configure suitable permissions such as `2770` when appropriate, and test access as each user.

---

# 23. Command Cheat Sheet

| Requirement | Command |
|---|---|
| Current effective user | `whoami` |
| User ID and groups | `id username` |
| Check current sessions | `who` |
| Show session activity | `w` |
| Look up user | `getent passwd username` |
| Look up group | `getent group groupname` |
| Create a user with home | `sudo useradd -m username` |
| Interactive Ubuntu account creation | `sudo adduser username` |
| Set password | `sudo passwd username` |
| Check password status | `sudo passwd -S username` |
| Check password aging | `sudo chage -l username` |
| Modify user | `sudo usermod ... username` |
| Create group | `sudo groupadd groupname` |
| Add supplementary group | `sudo usermod -aG groupname username` |
| Remove from group | `sudo gpasswd -d username groupname` |
| Rename group | `sudo groupmod -n newname oldname` |
| Delete group | `sudo groupdel groupname` |
| Delete user | `sudo userdel username` |
| Delete user and home | `sudo userdel -r username` |
| Switch user | `su - username` |
| Run one admin command | `sudo command` |
| Check sudo access | `sudo -l` |
| Change file owner | `sudo chown username file` |
| Change owner and group | `sudo chown username:groupname file` |
| Change group ownership | `sudo chgrp groupname file` |
| Check file metadata | `stat filename` |
| Check directory permissions | `ls -ld directory` |
| Find files owned by a user | `sudo find /home -user username -ls` |

---

# 24. Revision Checklist

Use this checklist after studying the topic.

- [ ] I can explain users, groups, UID, and GID.
- [ ] I understand root and sudo.
- [ ] I know the purpose of `/etc/passwd`, `/etc/shadow`, and `/etc/group`.
- [ ] I can inspect users using `id` and `getent`.
- [ ] I can create a test user and verify its home directory.
- [ ] I can set a password interactively.
- [ ] I can add a user to a supplementary group safely.
- [ ] I understand why `usermod -aG` matters.
- [ ] I can create and delete a test group.
- [ ] I understand the risks of `userdel -r`.
- [ ] I can explain `su` versus `sudo`.
- [ ] I can check administrator permissions with `sudo -l`.
- [ ] I understand `chown`, `chgrp`, and directory permissions.
- [ ] I know the common Ubuntu and RHEL-family admin group differences.
- [ ] I completed the EC2 hands-on lab.
- [ ] I completed or documented the DevOps team access project.
- [ ] I can answer the interview questions without looking at the notes.

## Final Memory Trick

- **User** = who is accessing the system.
- **UID** = numeric user identity.
- **Group** = users grouped for shared access.
- **GID** = numeric group identity.
- **`useradd`** = create a user.
- **`usermod`** = modify a user.
- **`userdel`** = delete a user.
- **`groupadd`** = create a group.
- **`id`** = verify user identity and groups.
- **`sudo`** = run an approved command with elevated privileges.
- **`chown`** = change owner.
- **`chgrp`** = change group.
- **`passwd`** = manage passwords.
- **`chage`** = manage password aging.

**Best learning method:** Read one section, practise the commands on a test machine, verify the output, write a short explanation in your own words, and revise the interview questions regularly.

