# Linux, Bash & Git Commands Reference

## Table of Contents
- [Directory Navigation](#directory-navigation)
- [File Management](#file-management)
- [File Viewing & Editing](#file-viewing--editing)
- [System Information](#system-information)
- [Package Management](#package-management)
- [Process Management](#process-management)
- [Bash Scripting Basics](#bash-scripting-basics)
- [Git Commands](#git-commands)
- [Important Safety Notes](#important-safety-notes)

---

## Directory Navigation

### Special Directory Symbols
- `/` - Root directory (top-level directory)
- `~` - Home directory of current user
- `.` - Current directory
- `..` - Parent directory (one level up)
- `-` - Previous directory

### Navigation Commands
- `cd` - Change directory
  - `cd /` - Go to root directory
  - `cd ~` or `cd` - Go to home directory
  - `cd ..` - Go up one level
  - `cd ../..` - Go up two levels
  - `cd -` - Go to previous directory
- `pwd` - Print working directory (show current location)

---

## File Management

### Creating Files & Directories
- `touch <filename>` - Create a new empty file
- `mkdir <directory>` - Create a new directory
  - `mkdir -p path/to/directory` - Create nested directories
  - `mkdir "directory name"` - Create directory with spaces in name

### Listing Files
- `ls` - List files in current directory
  - `ls -a` - List all files including hidden
  - `ls -l` - List in long format with details
  - `ls -al` or `ls -la` - List all in long format
  - `ls -lh` - List with human-readable file sizes
  - `ls -lt` - List sorted by modification time
  - `ls -ltr` - List sorted by time (reverse, oldest first)

### Copying, Moving & Deleting
- `cp <source> <destination>` - Copy file
  - `cp -r <source> <destination>` - Copy directory recursively
- `mv <source> <destination>` - Move file or directory
  - `mv <oldname> <newname>` - Rename file or directory
- `rm <filename>` - Delete file
  - `rm -r <directory>` - Delete directory and contents recursively ⚠️
  - `rm -rf <directory>` - Force delete without confirmation ⚠️
- `rmdir <directory>` - Delete empty directory

### File Permissions
- `chmod <permissions> <file>` - Change file permissions
  - `chmod +x <file>` - Make file executable
  - `chmod 755 <file>` - Set specific permissions (rwxr-xr-x)
  - `chmod -R 755 <directory>` - Change permissions recursively
- `chown <user>:<group> <file>` - Change file owner

---

## File Viewing & Editing

### Viewing File Contents
- `cat <filename>` - Display entire file contents
- `nl <filename>` - Display file with line numbers
- `head <filename>` - Show first 10 lines
  - `head -n <number> <filename>` - Show first n lines
- `tail <filename>` - Show last 10 lines
  - `tail -n <number> <filename>` - Show last n lines
  - `tail -f <filename>` - Follow file updates in real-time
- `less <filename>` - View file with scrolling (press `q` to quit)
- `more <filename>` - View file page by page

### Editing Files
- `nano <filename>` - Edit file in nano editor
  - `Ctrl+S` - Save
  - `Ctrl+X` - Exit
  - `Ctrl+K` - Cut line
  - `Ctrl+U` - Paste
- `vi <filename>` or `vim <filename>` - Edit in vi/vim editor
  - `i` - Enter insert mode
  - `Esc` - Exit insert mode
  - `:w` - Save
  - `:q` - Quit
  - `:wq` - Save and quit
  - `:q!` - Quit without saving

### Searching & Filtering
- `grep <pattern> <filename>` - Search for pattern in file
  - `grep -i <pattern> <filename>` - Case-insensitive search
  - `grep -r <pattern> <directory>` - Recursive search in directory
  - `grep -n <pattern> <filename>` - Show line numbers
- `cat <filename> | grep <pattern>` - Pipe file contents to grep
- `find <directory> -name <filename>` - Find files by name
  - `find . -type f -name "*.txt"` - Find all .txt files
  - `find . -type d -name <dirname>` - Find directories

---

## System Information

### Basic System Commands
- `uname` - Show operating system name
  - `uname -a` - Show all system information
  - `uname -r` - Show kernel version
  - `uname --help` - Show help for uname command
- `whoami` - Show current username
- `hostname` - Show system hostname
- `date` - Show current date and time
- `uptime` - Show system uptime
- `df -h` - Show disk space usage (human-readable)
- `du -sh <directory>` - Show directory size
- `free -h` - Show memory usage

### Command History & Help
- `history` - Show command history
  - `!<number>` - Execute command from history by number
  - `!!` - Execute last command
  - `!<command>` - Execute most recent command starting with <command>
- `man <command>` - Show manual page for command
- `<command> --help` - Show help for most commands
- `clear` or `Ctrl+L` - Clear terminal screen

---

## Package Management

### APT (Debian/Ubuntu)
- `sudo apt update` - Update package lists
  - `sudo apt update -y` - Update without confirmation
- `sudo apt upgrade` - Upgrade installed packages ⚠️
  - `sudo apt upgrade -y` - Upgrade without confirmation ⚠️
- `sudo apt install <package>` - Install package
  - `sudo apt install -y <package>` - Install without confirmation
- `sudo apt remove <package>` - Remove package
- `sudo apt autoremove` - Remove unused dependencies
- `sudo apt search <package>` - Search for package

### Other Commands
- `curl <url>` - Download file from URL
  - `curl -O <url>` - Download and save with original filename
- `wget <url>` - Download file from URL

---

## Process Management

### Viewing Processes
- `ps` - Show current processes
  - `ps aux` - Show all processes with detailed info
  - `ps -ef` - Show all processes in full format
- `top` - Show processes by CPU usage (interactive)
  - `q` - Quit top
- `htop` - Enhanced process viewer (if installed)
- `jobs` - Show background jobs in current shell

### Managing System Services
- `sudo systemctl status <service>` - Check service status
  - `Ctrl+C` or `q` - Exit status view
- `sudo systemctl start <service>` - Start service
- `sudo systemctl stop <service>` - Stop service
- `sudo systemctl restart <service>` - Restart service
- `sudo systemctl enable <service>` - Enable service at startup
- `sudo systemctl disable <service>` - Disable service at startup

### Process Control
- `<command> &` - Run command in background
- `Ctrl+C` - Stop current process
- `Ctrl+Z` - Suspend current process
- `bg` - Resume suspended process in background
- `fg` - Bring background process to foreground
- `kill <PID>` - Terminate process by PID ⚠️
- `kill -9 <PID>` - Force kill process ⚠️
- `killall <process_name>` - Kill all processes by name
- `sleep <seconds>` - Pause for specified seconds

---

## Bash Scripting Basics

### Script Execution
- `./<script.sh>` - Execute script in current directory
- `bash <script.sh>` - Execute script with bash
- `source <script.sh>` or `. <script.sh>` - Execute script in current shell

### Variables
```bash
# Assign variable
MY_VAR="value"

# Access variable
echo $MY_VAR
echo ${MY_VAR}

# Environment variables
export MY_VAR="value"
env                    # List all environment variables
echo $PATH             # Show PATH variable
```

### Common Bash Operators
```bash
# Shebang (first line of script)
#!/bin/bash

# Comments
# This is a comment

# Input/Output redirection
command > file         # Redirect output to file (overwrite)
command >> file        # Redirect output to file (append)
command < file         # Read input from file
command 2> file        # Redirect error output to file

# Piping
command1 | command2    # Pipe output of command1 to command2

# Conditional execution
command1 && command2   # Run command2 only if command1 succeeds
command1 || command2   # Run command2 only if command1 fails
```

### Useful Shortcuts
- `Ctrl+C` - Cancel current command
- `Ctrl+D` - Exit shell / End of input
- `Ctrl+L` - Clear screen
- `Ctrl+A` - Move cursor to beginning of line
- `Ctrl+E` - Move cursor to end of line
- `Ctrl+U` - Clear line before cursor
- `Ctrl+K` - Clear line after cursor
- `Ctrl+R` - Search command history
- `Tab` - Auto-complete commands and filenames

---

## Git Commands

### Initial Setup
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --list                           # View all settings
```

### Repository Basics
```bash
git init                                    # Initialize new repository
git clone <url>                             # Clone remote repository
git status                                  # Check repository status
git log                                     # View commit history
git log --oneline                           # View condensed commit history
git log --graph --oneline --all             # View branch graph
```

### Making Changes
```bash
git add <file>                              # Stage specific file
git add .                                   # Stage all changes
git add -A                                  # Stage all changes (including deletions)
git commit -m "message"                     # Commit staged changes
git commit -am "message"                    # Stage and commit tracked files
```

### Branches
```bash
git branch                                  # List local branches
git branch <branch-name>                    # Create new branch
git checkout <branch-name>                  # Switch to branch
git checkout -b <branch-name>               # Create and switch to new branch
git switch <branch-name>                    # Switch to branch (newer syntax)
git switch -c <branch-name>                 # Create and switch to new branch
git merge <branch-name>                     # Merge branch into current branch
git branch -d <branch-name>                 # Delete branch (safe)
git branch -D <branch-name>                 # Force delete branch
```

### Remote Repositories
```bash
git remote -v                               # List remote repositories
git remote add origin <url>                 # Add remote repository
git push origin <branch-name>               # Push to remote branch
git push -u origin <branch-name>            # Push and set upstream
git pull                                    # Fetch and merge from remote
git pull origin <branch-name>               # Pull specific branch
git fetch                                   # Download remote changes (no merge)
```

### Undoing Changes
```bash
git restore <file>                          # Discard changes in working directory
git restore --staged <file>                 # Unstage file
git reset HEAD~1                            # Undo last commit (keep changes)
git reset --hard HEAD~1                     # Undo last commit (discard changes) ⚠️
git revert <commit-hash>                    # Create new commit that undoes changes
```

### Other Useful Commands
```bash
git diff                                    # Show unstaged changes
git diff --staged                           # Show staged changes
git stash                                   # Temporarily save changes
git stash pop                               # Restore stashed changes
git stash list                              # List all stashes
git rm <file>                               # Remove file and stage deletion
git mv <old-name> <new-name>                # Rename/move file
```

---

## Important Safety Notes

### ⚠️ Dangerous Commands - Use With Caution

These commands can cause data loss or system issues. Always double-check before executing:

**File Deletion**
- `rm -r <directory>` - Deletes directory and ALL contents without warning
- `rm -rf <directory>` - Force deletes without any confirmation
- **Never** run `rm -rf /` or `rm -rf /*` - This will delete everything on the system!

**Privilege Escalation**
- `sudo su` - Logs in as root user with unlimited privileges
- `sudo <command>` - Executes command with elevated privileges
- Always verify commands before running with `sudo`

**Package Management**
- `sudo apt upgrade -y` - Upgrades all packages without confirmation - can break applications
- `sudo apt autoremove` - May remove needed dependencies
- Test upgrades in non-production environments first

**Process Management**
- `kill <PID>` - Terminates process immediately without cleanup
- `kill -9 <PID>` - Brute force kill, doesn't allow process cleanup
  - Can create zombie processes that consume resources
  - Doesn't kill child processes, leaving orphaned processes
  - Use as last resort only - try `kill <PID>` first

**Git Commands**
- `git reset --hard` - Permanently discards uncommitted changes
- `git push --force` - Overwrites remote history - can cause data loss for collaborators
- `git clean -fd` - Permanently deletes untracked files and directories

### Best Practices

1. **Always use `-i` flag for destructive operations** when available:
   - `rm -i <file>` - Prompts before deletion
   - `mv -i <source> <dest>` - Prompts before overwrite
   - `cp -i <source> <dest>` - Prompts before overwrite

2. **Test commands in safe environments** before production use

3. **Use version control** for important files

4. **Make backups** before major system changes

5. **Read command output** before confirming with `-y` flag

6. **Avoid running commands** you don't fully understand, especially with `sudo`
