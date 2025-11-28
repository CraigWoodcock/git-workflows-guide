# GitHub Repository Setup and Push Guide

A complete guide to creating a GitHub repository, initializing Git locally, and pushing your code to the remote repository using SSH.

## Prerequisites

This guide assumes you have already set up SSH authentication with GitHub. If you haven't, please follow this guide first:

👉 [SSH to GitHub Setup Guide](docs/ssh-setup.md)

## Table of Contents

- [Step 1: Create a Repository on GitHub](#step-1-create-a-repository-on-github)
- [Step 2: Copy the SSH URL](#step-2-copy-the-ssh-url)
- [Step 3: Initialize Git Locally](#step-3-initialize-git-locally)
- [Step 4: Connect to Remote Repository](#step-4-connect-to-remote-repository)
- [Step 5: Commit and Push Your Code](#step-5-commit-and-push-your-code)
- [Common Issues](#common-issues)

---

## Step 1: Create a Repository on GitHub

1. Navigate to [GitHub](https://github.com) and log in to your account
2. Click the **+** icon in the top-right corner
3. Select **New repository**
4. Fill in the repository details:
   - **Repository name**: Choose a descriptive name
   - **Description**: (Optional) Add a brief description
   - **Public/Private**: Select your preferred visibility
   - **Initialize repository**: Leave unchecked (we'll initialize locally)
5. Click **Create repository**

---

## Step 2: Copy the SSH URL

After creating your repository, you'll see a setup page with quick setup instructions.

1. Look for the section that says **Quick setup**
2. Click the **SSH** button (it may default to HTTPS)
3. Copy the SSH URL - it should look like this:
   ```
   git@github.com:username/repository-name.git
   ```

**Visual Guide:**

```
┌─────────────────────────────────────┐
│ Quick setup — if you've done this   │
│                                     │
│ HTTPS  [SSH] ← Click this           │
│ ┌─────────────────────────────────┐ │
│ │ git@github.com:user/repo.git   │ │ ← Copy this URL
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

---

## Step 3: Initialize Git Locally

Navigate to your project directory and initialize Git:

```bash
# Navigate to your project folder
cd /path/to/your/project

# Initialize a new Git repository
git init

# This creates a hidden .git folder that tracks your project's history
```

---

## Step 4: Connect to Remote Repository

Now establish the connection between your local repository and GitHub:

```bash
# Add the remote repository using the SSH URL you copied
git remote add origin git@github.com:username/repository-name.git

# 'origin' is the default name for the remote repository
# You can verify the connection with:
git remote -v
```

**Expected output:**
```
origin  git@github.com:username/repository-name.git (fetch)
origin  git@github.com:username/repository-name.git (push)
```

---

## Step 5: Commit and Push Your Code

Now you're ready to add, commit, and push your code:

```bash
# Add all files to staging area
git add .
# The '.' means "all files in current directory"
# You can also add specific files: git add filename.txt

# Commit your changes with a descriptive message
git commit -m "Initial commit"
# The -m flag allows you to add a commit message inline

# Rename the default branch to 'main' (if needed)
git branch -M main
# -M forces the rename even if the branch already exists

# Push your code to GitHub
git push -u origin main
# -u sets 'origin main' as the default upstream branch
# After this, you can use just 'git push' for future pushes
```

**Success!** Your code is now on GitHub. Visit your repository page to see your files.

---

## Common Issues

### Authentication Error

If you see:
```
Permission denied (publickey)
```

**Solution:** Your SSH key isn't properly configured. Follow the [SSH setup guide](https://github.com/CraigWoodcock/SSH-to-GitHub/tree/main/SSH%20to%20Github).

### Remote Already Exists

If you see:
```
fatal: remote origin already exists
```

**Solution:** Remove the existing remote and add it again:
```bash
# Remove the existing remote
git remote remove origin

# Add the remote again with the correct URL
git remote add origin git@github.com:username/repository-name.git
```

### Branch Name Mismatch

If your push is rejected due to branch names:

```bash
# Check your current branch name
git branch

# Rename your branch to main if needed
git branch -M main

# Push to the correct branch
git push -u origin main
```

---

## Quick Reference

**Basic Git Workflow:**

```bash
# Check status of your files
git status

# Add files to staging
git add .

# Commit changes
git commit -m "Your commit message"

# Push to GitHub
git push

# Pull latest changes from GitHub
git pull
```

---

## Additional Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [GitHub Documentation](https://docs.github.com)
- [SSH to GitHub Setup](https://github.com/CraigWoodcock/SSH-to-GitHub/tree/main/SSH%20to%20Github)

---
