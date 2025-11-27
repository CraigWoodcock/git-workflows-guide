# Git Workflows Guide

A comprehensive guide for Git, GitHub, and GitLab workflows, authentication, and automation.

## 📚 Table of Contents

1. [SSH Authentication Setup](#ssh-authentication-setup)
2. [GitHub Repository Management](#github-repository-management)
3. [GitLab Access Token Management](#gitlab-access-token-management)
4. [Command Reference](#command-reference)

---

## SSH Authentication Setup

Learn how to set up SSH keys for secure authentication with GitHub and GitLab.

**[📖 Read the full SSH setup guide →](docs/ssh-setup.md)**

### Quick Start
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your-email@example.com"

# Start SSH agent
eval "$(ssh-agent -s)"

# Add key to agent
ssh-add ~/.ssh/id_ed25519

# Test connection
ssh -T git@github.com
```

---

## GitHub Repository Management

Complete workflow for creating repositories and pushing code to GitHub.

**[📖 Read the full GitHub guide →](docs/github-setup.md)**

### Quick Start
```bash
# Initialize repository
git init

# Add remote
git remote add origin git@github.com:username/repo.git

# Commit and push
git add .
git commit -m "Initial commit"
git push -u origin main
```

---

## GitLab Access Token Management

Manage GitLab access tokens for GitHub Actions and automated workflows.

**[📖 Read the full GitLab token guide →](docs/gitlab-tokens.md)**

### Key Topics
- Creating access tokens
- Storing tokens securely
- Adding tokens to GitHub Secrets
- Renewing expired tokens
- Managing SSH keys for GitLab

---

## Command Reference

Quick reference for common Git, Linux, and Bash commands.

**[📖 View the command reference →](docs/command-reference.md)**

### Categories
- Git commands
- Linux file operations
- Bash scripting basics
- SSH utilities
- Text manipulation

---

## Repository Structure

```
git-workflows-guide/
├── README.md                      # This file
├── docs/
│   ├── ssh-setup.md              # SSH key generation and setup
│   ├── github-setup.md           # GitHub repository management
│   ├── gitlab-tokens.md          # GitLab access token guide
│   └── command-reference.md      # Git/Linux/Bash commands
├── examples/
│   ├── .bashrc-ssh-agent         # Auto-start SSH agent
│   ├── ssh-config-example        # SSH config template
│   └── github-workflow.yml       # GitHub Actions example
└── images/
    └── screenshots/              # Tutorial screenshots
```

---

## Getting Started

1. **New to SSH?** Start with the [SSH Setup Guide](docs/ssh-setup.md)
2. **Creating a repository?** Check the [GitHub Setup Guide](docs/github-setup.md)
3. **Setting up automation?** See the [GitLab Token Guide](docs/gitlab-tokens.md)
4. **Need a command?** Browse the [Command Reference](docs/command-reference.md)

---

## Contributing

Found an error or want to improve the documentation? Feel free to:
- Open an issue
- Submit a pull request
- Suggest improvements

---

## Quick Links

- [GitHub Documentation](https://docs.github.com)
- [GitLab Documentation](https://docs.gitlab.com)
- [Git Documentation](https://git-scm.com/doc)
- [OpenSSH Documentation](https://www.openssh.com/manual.html)

---
