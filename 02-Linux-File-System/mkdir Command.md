# mkdir Command - Linux

## What is mkdir?
mkdir means Make Directory. It creates directories in Linux.

## Syntax
```bash
mkdir [OPTIONS] DIRECTORY
```

## Basic Examples

### Create one directory
```bash
mkdir devops
```

### Create multiple directories
```bash
mkdir aws docker kubernetes
```

### Create parent and child directories
```bash
mkdir -p projects/linux/scripts
```

### Verbose mode
```bash
mkdir -v terraform
```

### Set directory permissions
```bash
mkdir -m 700 private
ls -ld private
```

### Combine options
```bash
mkdir -pv project/scripts/logs
```

## EC2 Hands-on Practice
```bash
cd ~
mkdir -p linux-practice
cd linux-practice
mkdir -v test
mkdir -p app/config/logs
ls -R
```

## Interview Questions
1. What is mkdir?
2. What is the difference between mkdir and mkdir -p?
3. What does mkdir -m 700 mean?
4. What is verbose mode?

## Memory Tricks
- p = Parents
- v = Verbose
- m = Mode (permissions)

## Practice Status
- [ ] Read the command
- [ ] Practice on EC2
- [ ] Understand the output
- [ ] Revise tomorrow
