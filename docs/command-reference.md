# Command Reference Guide

A comprehensive reference of essential Git, Linux, and Bash commands for daily development work.

---

## Git Commands

### Repository Setup

```bash
# Initialize a new Git repository
git init

# Clone an existing repository
git clone <repository-url>
git clone git@github.com:username/repo.git

# Clone a specific branch
git clone -b <branch-name> <repository-url>
```

### Configuration

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# View all configuration
git config --list

# Set default branch name
git config --global init.defaultBranch main

# Configure line endings (Windows)
git config --global core.autocrlf true

# Configure line endings (Mac/Linux)
git config --global core.autocrlf input
```

### Basic Workflow

```bash
# Check repository status
git status

# Add files to staging area
git add <file>
git add .                    # Add all files
git add *.js                 # Add all JS files
git add -p                   # Interactive staging

# Commit changes
git commit -m "Commit message"
git commit -am "Message"     # Add and commit tracked files
git commit --amend           # Modify last commit

# View commit history
git log
git log --oneline            # Compact view
git log --graph              # Visual branch graph
git log -n 5                 # Last 5 commits
git log --author="Name"      # Filter by author
```

### Branching

```bash
# List branches
git branch                   # Local branches
git branch -r                # Remote branches
git branch -a                # All branches

# Create a new branch
git branch <branch-name>

# Switch to a branch
git checkout <branch-name>
git switch <branch-name>     # Modern alternative

# Create and switch to new branch
git checkout -b <branch-name>
git switch -c <branch-name>

# Delete a branch
git branch -d <branch-name>  # Safe delete
git branch -D <branch-name>  # Force delete

# Rename current branch
git branch -m <new-name>
```

### Merging

```bash
# Merge branch into current branch
git merge <branch-name>

# Abort a merge
git merge --abort

# Continue after resolving conflicts
git merge --continue
```

### Remote Repositories

```bash
# View remote repositories
git remote -v

# Add a remote repository
git remote add origin <repository-url>

# Change remote URL
git remote set-url origin <new-url>

# Remove a remote
git remote remove <remote-name>

# Fetch changes from remote
git fetch
git fetch origin

# Pull changes (fetch + merge)
git pull
git pull origin main

# Push changes to remote
git push
git push origin main
git push -u origin main      # Set upstream and push
git push --all               # Push all branches
git push --tags              # Push all tags

# Push a new branch
git push -u origin <branch-name>
```

### Undoing Changes

```bash
# Discard changes in working directory
git checkout -- <file>
git restore <file>           # Modern alternative

# Unstage files
git reset HEAD <file>
git restore --staged <file>

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Revert a commit (creates new commit)
git revert <commit-hash>

# Clean untracked files
git clean -n                 # Dry run
git clean -f                 # Remove files
git clean -fd                # Remove files and directories
```

### Stashing

```bash
# Save changes temporarily
git stash
git stash save "Description"

# List stashes
git stash list

# Apply most recent stash
git stash apply
git stash pop                # Apply and remove

# Apply specific stash
git stash apply stash@{2}

# Delete stash
git stash drop stash@{0}
git stash clear              # Delete all stashes
```

### Tagging

```bash
# List tags
git tag

# Create a tag
git tag v1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"

# Push tags to remote
git push origin v1.0.0
git push --tags

# Delete a tag
git tag -d v1.0.0                    # Local
git push origin --delete v1.0.0     # Remote
```

### Viewing Changes

```bash
# Show changes in working directory
git diff

# Show staged changes
git diff --staged
git diff --cached

# Show changes between branches
git diff branch1..branch2

# Show changes in a commit
git show <commit-hash>
```

---

## Linux/Bash Commands

### Navigation

```bash
# Print working directory
pwd

# List files
ls
ls -l                        # Long format
ls -la                       # Include hidden files
ls -lh                       # Human readable sizes
ls -ltr                      # Sort by time, reverse

# Change directory
cd /path/to/directory
cd ..                        # Parent directory
cd ~                         # Home directory
cd -                         # Previous directory
```

### File Operations

```bash
# Create file
touch filename.txt

# Create directory
mkdir directory-name
mkdir -p path/to/nested/dir  # Create parent directories

# Copy files/directories
cp file1.txt file2.txt
cp -r dir1 dir2              # Copy directory recursively

# Move/rename files
mv oldname.txt newname.txt
mv file.txt /path/to/destination/

# Remove files/directories
rm file.txt
rm -r directory              # Remove directory
rm -rf directory             # Force remove (be careful!)

# View file contents
cat file.txt                 # Entire file
less file.txt                # Paginated view
head file.txt                # First 10 lines
tail file.txt                # Last 10 lines
tail -f file.txt             # Follow file (logs)
```

### File Searching

```bash
# Find files by name
find . -name "*.txt"
find /path -type f -name "pattern"

# Search file contents
grep "search-term" file.txt
grep -r "term" directory/    # Recursive search
grep -i "term" file.txt      # Case insensitive
grep -n "term" file.txt      # Show line numbers
```

### Permissions

```bash
# Change file permissions
chmod 755 script.sh
chmod +x script.sh           # Make executable
chmod -x script.sh           # Remove executable

# Change file owner
chown user:group file.txt
chown -R user:group directory/

# View permissions
ls -l
```

### System Information

```bash
# Display current user
whoami

# Display system info
uname -a

# Display disk usage
df -h

# Display directory size
du -sh directory/
du -h --max-depth=1

# Display running processes
ps aux
top
htop                         # Interactive (if installed)

# Display memory usage
free -h
```

### Text Processing

```bash
# Count lines, words, characters
wc file.txt
wc -l file.txt               # Lines only

# Sort lines
sort file.txt
sort -r file.txt             # Reverse order
sort -n file.txt             # Numeric sort

# Remove duplicates
sort file.txt | uniq

# Stream editor
sed 's/old/new/g' file.txt   # Replace text
sed -i 's/old/new/g' file.txt  # Edit in place

# Advanced text processing
awk '{print $1}' file.txt    # Print first column
```

### Compression

```bash
# Create tar archive
tar -czf archive.tar.gz directory/

# Extract tar archive
tar -xzf archive.tar.gz

# Create zip archive
zip -r archive.zip directory/

# Extract zip archive
unzip archive.zip
```

### Network Commands

```bash
# Download files
wget https://example.com/file.zip
curl -O https://example.com/file.zip

# Check network connectivity
ping google.com
ping -c 4 google.com         # Send 4 packets

# Display network connections
netstat -tuln
ss -tuln                     # Modern alternative

# DNS lookup
nslookup example.com
dig example.com
```

### Process Management

```bash
# Run command in background
command &

# List background jobs
jobs

# Bring job to foreground
fg
fg %1

# Kill process
kill PID
kill -9 PID                  # Force kill
killall process-name

# Find process ID
pidof process-name
pgrep process-name
```

### SSH Commands

```bash
# Connect to remote server
ssh user@hostname
ssh -p 2222 user@hostname    # Custom port

# Copy files to remote (SCP)
scp file.txt user@host:/path/
scp -r directory/ user@host:/path/

# Copy files to remote (RSYNC)
rsync -avz file.txt user@host:/path/
rsync -avz --delete dir/ user@host:/path/

# Generate SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

# Copy SSH key to server
ssh-copy-id user@hostname

# Start SSH agent
eval "$(ssh-agent -s)"

# Add SSH key to agent
ssh-add ~/.ssh/id_ed25519
```

---

## Bash Scripting Basics

### Variables

```bash
# Define variable
NAME="John"
AGE=25

# Use variable
echo $NAME
echo "Hello, $NAME"
echo "Hello, ${NAME}"        # Preferred syntax
```

### Conditionals

```bash
# If statement
if [ condition ]; then
    echo "True"
fi

# If-else
if [ condition ]; then
    echo "True"
else
    echo "False"
fi

# File checks
if [ -f "file.txt" ]; then
    echo "File exists"
fi

if [ -d "directory" ]; then
    echo "Directory exists"
fi
```

### Loops

```bash
# For loop
for i in 1 2 3 4 5; do
    echo $i
done

# While loop
while [ condition ]; do
    echo "Running"
done

# Loop through files
for file in *.txt; do
    echo $file
done
```

### Functions

```bash
# Define function
my_function() {
    echo "Hello from function"
}

# Call function
my_function

# Function with parameters
greet() {
    echo "Hello, $1"
}

greet "John"
```

---

## Useful Command Combinations

```bash
# Find and delete all .log files
find . -name "*.log" -type f -delete

# Count number of files in directory
ls -1 | wc -l

# Find largest files
du -ah . | sort -rh | head -n 10

# Monitor git status continuously
watch -n 2 git status

# Create backup with timestamp
cp file.txt file.txt.$(date +%Y%m%d_%H%M%S)

# Search and replace in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} +

# Git: View files changed in last commit
git diff-tree --no-commit-id --name-only -r HEAD

# Git: Undo all uncommitted changes
git reset --hard && git clean -fd

# Check which process is using a port
lsof -i :8080
netstat -tuln | grep 8080
```

---

## Keyboard Shortcuts (Bash Terminal)

```
Ctrl + C        Cancel current command
Ctrl + D        Exit terminal / End of file
Ctrl + L        Clear screen (same as 'clear')
Ctrl + A        Move to beginning of line
Ctrl + E        Move to end of line
Ctrl + U        Delete from cursor to beginning
Ctrl + K        Delete from cursor to end
Ctrl + W        Delete word before cursor
Ctrl + R        Search command history
Ctrl + Z        Suspend current process
Tab             Auto-complete
!!              Repeat last command
```

---

## Tips & Best Practices

1. **Use Tab completion**: Save time by using Tab to auto-complete commands and paths
2. **Check before force**: Always review before using `-f` or `--force` flags
3. **Backup important data**: Before running destructive commands
4. **Use aliases**: Create shortcuts for frequently used commands in `.bashrc`
5. **Read man pages**: Use `man command` to learn more about any command
6. **Test in dry-run**: Many commands have `-n` or `--dry-run` options
7. **Use version control**: Always commit your work regularly
8. **Write clear commit messages**: Future you will thank present you

---

## Quick Reference Card

| Task | Command |
|------|---------|
| Clone repo | `git clone <url>` |
| Check status | `git status` |
| Stage files | `git add .` |
| Commit | `git commit -m "message"` |
| Push | `git push` |
| Pull | `git pull` |
| Create branch | `git checkout -b <name>` |
| Switch branch | `git checkout <name>` |
| View history | `git log --oneline` |
| Undo changes | `git restore <file>` |
| List files | `ls -la` |
| View file | `cat <file>` |
| Search text | `grep "text" <file>` |
| Find files | `find . -name "*.txt"` |
| Check processes | `ps aux` |
| SSH connect | `ssh user@host` |

---

For more detailed information on any command, use:
```bash
man <command>          # Manual page
<command> --help       # Quick help
```
