# SSH to GitHub

## Table of Contents

- [What Are SSH Keys?](#what-are-ssh-keys)
- [Prerequisites](#prerequisites)
- [Generate SSH Keys](#generate-ssh-keys)
- [Locate and Verify Keys](#locate-and-verify-keys)
- [Register Public Key on GitHub](#register-public-key-on-github)
- [Start the SSH Agent](#start-the-ssh-agent)
- [Add Private Key to SSH Agent](#add-private-key-to-ssh-agent)
- [Test the SSH Connection](#test-the-ssh-connection)
- [Create a Test Git Repository](#create-a-test-git-repository)
- [Make SSH Keys Persist After Reboot](#make-ssh-keys-persist-after-reboot)
- [Create SSH Config File](#create-ssh-config-file)
- [Final Connection Test](#final-connection-test)
- [Optional Improvements](#optional-improvements)
- [Troubleshooting](#troubleshooting)

---

## What Are SSH Keys?

SSH (Secure Shell) is a cryptographic network protocol that provides a secure way to access remote systems and services. SSH keys are a pair of cryptographic keys used to authenticate your identity when connecting to GitHub.

![SSH Key Pairs](<../images/ssh_key_pairs.jpeg>)

**How SSH Keys Work:**
1. **Private Key**: Stays on your local machine (never share this!)
2. **Public Key**: Uploaded to GitHub (safe to share)
3. When you connect, GitHub uses your public key to verify you have the matching private key

**Benefits of SSH over HTTPS:**
- No need to enter username/password for each push/pull
- More secure authentication
- Required for many automation workflows

---

## Prerequisites

Before starting, ensure you have:

- Git Bash or PowerShell installed on Windows
- A GitHub account
- Administrator access to your computer (for configuring services)

---

## Generate SSH Keys

SSH keys are generated using the `ssh-keygen` command-line tool.

1. Open Git Bash terminal

2. Navigate to the `.ssh` directory:
   ```bash
   cd ~/.ssh
   ```

3. Generate a new SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "email@youremail.com"
   ```
   
   **Command breakdown:**
   - `-t rsa` = Key type (RSA algorithm)
   - `-b 4096` = Key size in bits (4096 is highly secure)
   - `-C "email@youremail.com"` = Comment (typically your email)

4. When prompted, enter a name for your key files:
   ```
   craig-github-ssh-key
   ```
   > **Tip**: Name keys based on their purpose (e.g., `work-github-key`, `personal-gitlab-key`)

5. Press Enter through the passphrase prompts (or add a passphrase for extra security)

6. You should see output similar to this:
   ![RSA KEY](<../images/rsa_fingerprint.png>)

---

## Locate and Verify Keys

After generation, verify your keys were created successfully.

1. List files in the `.ssh` directory:
   ```bash
   ls
   ```

2. You should see two files:
   - `craig-github-ssh-key` (private key - keep this secret!)
   - `craig-github-ssh-key.pub` (public key - safe to share)

3. Display your public key contents:
   ```bash
   cat craig-github-ssh-key.pub
   ```

4. Copy the entire output (starts with `ssh-rsa` and ends with your email):
   ![Public key](<../SSH Screenshots/Screenshot 2024-01-08 150705.png>)

---

## Register Public Key on GitHub

Add your public key to your GitHub account to establish the authentication connection.

1. Go to [GitHub.com](https://github.com) and sign in

2. Click on your profile picture (top-right corner)

3. Select **Settings**

4. In the left sidebar, click **SSH and GPG keys**

5. Click the **New SSH key** button

6. Fill in the form:
   - **Title**: Use the same name as your key file (e.g., `craig-github-ssh-key`)
   - **Key**: Paste the public key you copied earlier

7. Click **Add SSH key**

   ![SSH Github](<../SSH Screenshots/Screenshot 2024-01-08 151034.png>)

8. You may be prompted to confirm with your GitHub password

---

## Start the SSH Agent

The SSH agent manages your SSH keys and handles authentication in the background.

1. In Git Bash, start the SSH agent:
   ```bash
   eval $(ssh-agent -s)
   ```

2. You should see output like:
   ```
   Agent pid 1234
   ```

> **Note**: The SSH agent needs to be running before you can add keys to it.

---

## Add Private Key to SSH Agent

Load your private key into the SSH agent so it can be used for authentication.

1. Add your private key:
   ```bash
   ssh-add craig-github-ssh-key
   ```

2. You should see confirmation:
   ```
   Identity added: craig-github-ssh-key (email@youremail.com)
   ```

3. Verify the key was added:
   ```bash
   ssh-add -l
   ```

4. This will list all keys currently loaded in the agent

---

## Test the SSH Connection

Verify that GitHub recognizes your SSH key and can authenticate you.

1. Navigate out of the `.ssh` folder:
   ```bash
   cd ..
   ```

2. Test the connection:
   ```bash
   ssh -T git@github.com
   ```

3. If this is your first connection, you'll see a warning about host authenticity:
   ```
   The authenticity of host 'github.com' can't be established.
   Are you sure you want to continue connecting (yes/no)?
   ```
   Type `yes` and press Enter

4. You should see a success message:
   ```
   Hi username! You've successfully authenticated, but GitHub does not provide shell access.
   ```

> **Note**: The message about "does not provide shell access" is normal - GitHub doesn't allow interactive SSH sessions, only Git operations.

---

## Create a Test Git Repository

Test your SSH configuration by creating and pushing a repository.

1. Create a new test directory:
   ```bash
   mkdir test-ssh-repo
   cd test-ssh-repo
   ```

2. Initialize a Git repository:
   ```bash
   git init
   ```

3. Create a test file:
   ```bash
   echo "This is a test file" > testFile.txt
   ```

4. Create a repository on GitHub:
   - Go to GitHub.com
   - Click the **+** icon (top-right)
   - Select **New repository**
   - Name it `test-ssh-repo`
   - Do NOT initialize with README
   - Click **Create repository**

5. Add the GitHub repository as a remote:
   ```bash
   git remote add origin git@github.com:username/test-ssh-repo.git
   ```
   > **Important**: Replace `username` with your GitHub username

6. Stage, commit, and push your changes:
   ```bash
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git push -u origin main
   ```

7. Verify your file appears on GitHub in your repository

---

## Make SSH Keys Persist After Reboot

By default, SSH keys need to be re-added to the agent every time you restart your computer. Let's configure Windows to handle this automatically.

### Configure SSH Agent Service

1. Open **PowerShell as Administrator**:
   - Search for "PowerShell" in Windows
   - Right-click and select "Run as Administrator"

2. Set the SSH Agent service to start automatically:
   ```powershell
   Get-Service -Name ssh-agent | Set-Service -StartupType Automatic
   ```

3. Start the SSH Agent service:
   ```powershell
   Start-Service ssh-agent
   ```

4. Verify the service is running:
   ```powershell
   Get-Service -Name ssh-agent
   ```
   
   You should see:
   ```
   Status: Running
   StartType: Automatic
   ```

### Add Your Key to the Agent

1. Return to Git Bash and add your key (one-time setup):
   ```bash
   ssh-add ~/.ssh/craig-github-ssh-key
   ```

2. Verify the key is loaded:
   ```bash
   ssh-add -l
   ```

---

## Create SSH Config File

The SSH config file automatically tells SSH which key to use for different hosts, eliminating the need to specify keys manually.

1. Navigate to your `.ssh` directory:
   ```bash
   cd ~/.ssh
   ```

2. Create or edit the config file:
   ```bash
   notepad config
   ```
   > **Alternative editors**: `vim config` or `nano config`

3. Add the following configuration:
   ```
   # GitHub Configuration
   Host github.com
       HostName github.com
       User git
       IdentityFile ~/.ssh/craig-github-ssh-key
       IdentitiesOnly yes
       AddKeysToAgent yes
   ```

   **Configuration breakdown:**
   - `Host github.com` = Applies to GitHub connections
   - `HostName github.com` = The actual server address
   - `User git` = Always use "git" as the user
   - `IdentityFile` = Path to your private key
   - `IdentitiesOnly yes` = Only use specified key (not others)
   - `AddKeysToAgent yes` = Auto-add key to agent when used

4. Save and close the file

5. Set proper permissions (in Git Bash):
   ```bash
   chmod 600 config
   ```

---

## Final Connection Test

Test that everything is configured correctly and working automatically.

1. Close all terminal windows

2. Open a new Git Bash terminal

3. Test GitHub connection:
   ```bash
   ssh -T git@github.com
   ```

4. You should successfully authenticate without manually adding keys

5. Verify your key is automatically loaded:
   ```bash
   ssh-add -l
   ```

**Success!** Your SSH keys will now:
- Load automatically when needed
- Persist across terminal sessions
- Survive system reboots

---

## Optional Improvements

### Adding Multiple Keys for Different Services

You can configure SSH keys for multiple services (GitHub, GitLab, Bitbucket, etc.)

Add additional `Host` blocks to your `~/.ssh/config` file:

```
# GitHub Configuration
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/craig-github-ssh-key
    IdentitiesOnly yes
    AddKeysToAgent yes

# GitLab Configuration
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/craig-gitlab-ssh-key
    IdentitiesOnly yes
    AddKeysToAgent yes

# Personal GitHub Account (using alias)
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/personal-github-ssh-key
    IdentitiesOnly yes
    AddKeysToAgent yes
```

Then clone repositories using the host alias:
```bash
git clone git@github-personal:username/repo.git
```

### Adding a Passphrase to Your Key

For additional security, you can add a passphrase to your SSH key:

1. Add passphrase to existing key:
   ```bash
   ssh-keygen -p -f ~/.ssh/craig-github-ssh-key
   ```

2. Enter new passphrase when prompted

3. Configure SSH agent to remember passphrase (Windows):
   - The Windows SSH agent will cache your passphrase after first use
   - You'll only need to enter it once per session

### Using SSH Keys with Git GUI Tools

Most Git GUI tools (GitKraken, GitHub Desktop, SourceTree) can use your SSH keys:

1. Ensure your SSH agent service is running
2. The GUI tool will automatically detect and use your loaded keys
3. Some tools may require you to specify the key location in settings

---

## Troubleshooting

### SSH Agent Service Issues

**Problem**: SSH Agent service won't start or keeps stopping

**Solutions**:
1. Verify the service exists and check its status:
   ```powershell
   Get-Service -Name ssh-agent
   ```

2. If the service is disabled, enable it:
   ```powershell
   Set-Service -Name ssh-agent -StartupType Automatic
   Start-Service ssh-agent
   ```

3. Check if another process is using the SSH Agent:
   ```powershell
   Get-Process | Where-Object {$_.ProcessName -like "*ssh*"}
   ```

4. Restart the service:
   ```powershell
   Restart-Service ssh-agent
   ```

### Key Not Persisting After Reboot

**Problem**: SSH keys are not loaded after restarting your computer

**Solutions**:
1. Verify SSH Agent is set to auto-start:
   ```powershell
   Get-Service -Name ssh-agent | Select-Object StartType
   ```
   - Should show `StartType: Automatic`

2. Check if your key is in the agent:
   ```bash
   ssh-add -l
   ```

3. If empty, add the key manually once:
   ```bash
   ssh-add ~/.ssh/craig-github-ssh-key
   ```

4. Verify your SSH config file has `AddKeysToAgent yes`:
   ```bash
   cat ~/.ssh/config
   ```

5. Try connecting to GitHub to trigger automatic key loading:
   ```bash
   ssh -T git@github.com
   ```

### Permission Denied Errors

**Problem**: Getting "Permission denied (publickey)" when trying to connect

**Solutions**:
1. Verify your public key is registered on GitHub:
   - Go to GitHub → Settings → SSH and GPG keys
   - Confirm your key is listed

2. Check if the correct key is being used:
   ```bash
   ssh -vT git@github.com
   ```
   - Look for "Offering public key" in the output

3. Verify key file permissions (in Git Bash):
   ```bash
   chmod 600 ~/.ssh/craig-github-ssh-key
   chmod 600 ~/.ssh/config
   ```

4. Test if your private key matches the public key on GitHub:
   ```bash
   ssh-keygen -y -f ~/.ssh/craig-github-ssh-key
   ```
   - Compare output with your `.pub` file

5. Ensure you're using the git user:
   ```bash
   ssh -T git@github.com
   ```
   - NOT your GitHub username

### Config File Not Working

**Problem**: SSH config file isn't being recognized or keys aren't auto-loading

**Solutions**:
1. Verify config file location and name:
   ```bash
   ls -la ~/.ssh/config
   ```
   - File must be named `config` (no extension)

2. Check config file syntax:
   ```bash
   cat ~/.ssh/config
   ```
   - Verify proper indentation (4 spaces or 1 tab)
   - Ensure no extra spaces at end of lines

3. Test config file:
   ```bash
   ssh -T -v git@github.com
   ```
   - Look for "Reading configuration data" in verbose output

4. Verify file permissions:
   ```bash
   chmod 600 ~/.ssh/config
   ```

5. Check if IdentityFile path is correct:
   ```bash
   ls -la ~/.ssh/craig-github-ssh-key
   ```

### General Debugging Tips

1. **Use verbose mode** to see detailed connection information:
   ```bash
   ssh -vvv git@github.com
   ```

2. **Clear SSH known_hosts** if you're getting host key verification errors:
   ```bash
   ssh-keygen -R github.com
   ```

3. **Verify Git is using SSH** and not HTTPS:
   ```bash
   git remote -v
   ```
   - URLs should start with `git@github.com:` not `https://`

4. **Check Windows Firewall** isn't blocking SSH:
   - Open Windows Defender Firewall
   - Check if SSH (port 22) is allowed

5. **Regenerate keys** if all else fails:
   - Back up your old keys
   - Follow the [Generate SSH Keys](#generate-ssh-keys) section again
   - Update the config file with the new key name
