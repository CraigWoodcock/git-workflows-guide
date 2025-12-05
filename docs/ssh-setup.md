# SSH to GitHub

SSH (Secure Shell) is a secure method for authenticating with remote systems such as GitHub.  
Once configured, you can push and pull code from GitHub using your SSH keys instead of passwords.

---

# Table of Contents
1. [What Are SSH Keys?](#what-are-ssh-keys)  
2. [Prerequisites](#prerequisites)  
3. [Generate SSH Keys](#generate-ssh-keys)  
4. [Locate and Verify Keys](#locate-and-verify-keys)  
5. [Register Public Key on GitHub](#register-public-key-on-github)  
6. [Start the SSH Agent](#start-the-ssh-agent)  
7. [Add Private Key to SSH Agent](#add-private-key-to-ssh-agent)  
8. [Test the SSH Connection](#test-the-ssh-connection)  
9. [Create a Test Git Repository](#create-a-test-git-repository)  
10. [Make SSH Keys Persist After Reboot](#make-ssh-keys-persist-after-reboot)  
11. [Create SSH Config File](#create-ssh-config-file)  
12. [Final Connection Test](#final-connection-test)  
13. [Optional Improvements](#optional-improvements)

---

# 1. What Are SSH Keys?

SSH keys are a pair of cryptographic files:

- **Private key** (kept on your machine)
- **Public key** (uploaded to GitHub)

Your system proves identity using the **private key**, while GitHub verifies it using the **public key**.

![SSH Key Pairs](../images/ssh_key_pairs.jpeg)

---

# 2. Prerequisites

- Git installed  
- Git Bash terminal  
- GitHub account  

---

# 3. Generate SSH Keys

1. Open Git Bash and move into your SSH directory:

```bash
cd ~/.ssh
```

2. Generate a new key pair (RSA example):

```bash
ssh-keygen -t rsa -b 4096 -C "email@youremail.com"
```

3. Name your key files, e.g.:

```
craig-github-ssh-key
```

4. Press Enter through the prompts.

![RSA KEY](../images/rsa_fingerprint.png)

---

# 4. Locate and Verify Keys

```bash
ls
```

Show the public key:

```bash
cat craig-github-ssh-key.pub
```

![Public key](../SSH Screenshots/Screenshot 2024-01-08 150705.png)

---

# 5. Register Public Key on GitHub

Navigate to:

**Profile â†’ Settings â†’ SSH and GPG Keys â†’ New SSH Key**

![SSH Github](../SSH Screenshots/Screenshot 2024-01-08 151034.png)

---

# 6. Start the SSH Agent

```bash
eval $(ssh-agent -s)
```

---

# 7. Add Private Key to SSH Agent

```bash
ssh-add craig-github-ssh-key
```

---

# 8. Test the SSH Connection

```bash
cd ..
ssh -T git@github.com
```

---

# 9. Create a Test Git Repository

```bash
mkdir test-ssh-repo
cd test-ssh-repo
echo "this is a test line" > testFile.txt
```

Add remote:

```bash
git remote add origin git@github.com:USERNAME/REPO.git
```

Commit & push:

```bash
git add .
git commit -m "Add test file"
git push origin main
```

---

# 10. Make SSH Keys Persist After Reboot

PowerShell (Admin):

```powershell
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
Get-Service ssh-agent
```

---

# 11. Create SSH Config File

```bash
cd ~/.ssh
notepad config
```

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/craig-github-ssh-key
    IdentitiesOnly yes
    AddKeysToAgent yes
```

Permissions:

```bash
chmod 600 config
```

---

# 12. Final Connection Test

```bash
ssh -T git@github.com
```

---

# 13. Optional Improvements

### Ed25519 (recommended)

```bash
ssh-keygen -t ed25519 -C "email@youremail.com"
```

---
