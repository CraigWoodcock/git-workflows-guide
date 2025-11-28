# GitLab Access Token Management for GitHub Actions

Complete guide for creating, storing, and managing GitLab access tokens used in GitHub Actions workflows.

> **📝 Need to generate a new SSH key pair?**  
> View the detailed guide here: [SSH Key Generation Documentation](./ssh-setup.md)

---

## Table of Contents
- [Creating a GitLab Access Token](#creating-a-gitlab-access-token)
- [Storing the Token Securely](#storing-the-token-securely)
- [Creating the GitHub Secret](#creating-the-github-secret)
- [Updating an Expired Token](#updating-an-expired-token)
- [Renewing SSH Keys for GitLab](#renewing-ssh-keys-for-gitlab)
- [Troubleshooting](#troubleshooting)

---

## Creating a GitLab Access Token

### Step 1: Access the Token Settings
1. Log in to [GitLab.com](https://gitlab.com)
2. Click on your **avatar** in the top-right corner
3. Select **Settings** (or **Preferences**)
4. In the left sidebar, click on **Access Tokens**

### Step 2: Generate a New Token
1. Click the **Add new token** button
2. Configure your token:
   - **Token name**: Give it a descriptive name
     - Example: `GitHub Actions Sync Token`
   - **Expiration date**: Set an expiration date
     - Recommended: 1 year from creation date
     - Note this date for future renewal
   - **Select scopes**: Choose the appropriate permissions
     - ✅ **`write_repository`** - Required for pushing code
     - ✅ **`read_repository`** - Optional, if you need to pull code
     - ✅ **`api`** - Optional, for full API access

3. Click **Create personal access token**

### Step 3: Copy Your Token
⚠️ **IMPORTANT**: Your token will only be displayed once!

The token will look like: `glpat-xxxxxxxxxxxxxxxxxxxx`

Copy it immediately and proceed to the next section.

---

## Storing the Token Securely

### Option 1: Password Manager (Recommended)
Store your token in a password manager like:
- 1Password
- Bitwarden
- LastPass
- Apple Keychain

**Save the following details:**
- Token value: `glpat-xxxxxxxxxxxxxxxxxxxx`
- Token name: `GitHub Actions Sync Token`
- Created date: `YYYY-MM-DD`
- Expiration date: `YYYY-MM-DD`
- Scope: `write_repository`
- Purpose: `GitHub to GitLab sync`

### Option 2: Store in .ssh Directory
If you don't use a password manager, you can store token details in your `.ssh` directory alongside your SSH keys:
1. Create a secure text file in `~/.ssh/`:
   ```bash
   nano ~/.ssh/gitlab_tokens.txt
   ```
2. Add your token details:
   ```
   GitLab Access Token - GitHub Actions Sync
   Token: glpat-xxxxxxxxxxxxxxxxxxxx
   Created: YYYY-MM-DD
   Expires: YYYY-MM-DD
   Scope: write_repository
   Purpose: GitHub to GitLab sync
   ```
3. Secure the file permissions:
   ```bash
   chmod 600 ~/.ssh/gitlab_tokens.txt
   ```
4. Set a calendar reminder for token expiration

**Benefits:**
- The `.ssh` directory is hidden by default on Unix-based systems
- It's already used for storing sensitive SSH keys
- Proper permissions (600) ensure only you can read it
- Keeps all authentication credentials in one place

**Note:** Once you've added the access token to GitHub and confirmed the workflow works successfully, you can delete the local `gitlab_tokens.txt` file if desired. The token is only needed by GitHub Actions (stored securely in GitHub Secrets), not by your local machine. However, keeping a record of the token details (creation date, expiration date, scope) can be helpful for future reference and token management.

### Option 3: Secure Note
If neither option above works for you:
1. Create a secure note in a safe location
2. Include all the details listed above
3. Set a calendar reminder for token expiration

⚠️ **Never:**
- Commit tokens to Git repositories
- Share tokens via email or chat
- Store tokens in plain text files
- Leave tokens in your clipboard history

---

## Creating the GitHub Secret

Once you have your GitLab token, you need to add it as a secret in GitHub so your workflows can use it.

### Step 1: Navigate to Repository Settings
1. Go to your GitHub repository
2. Click on the **Settings** tab
3. In the left sidebar, expand **Secrets and variables**
4. Click on **Actions**

### Step 2: Create New Secret
1. Click the **New repository secret** button
2. Configure the secret:
   - **Name**: `GITLAB_TOKEN`
     - Use this exact name if your workflow references it
     - Or use a custom name and update your workflow accordingly
   - **Secret**: Paste your GitLab access token
     - Example: `glpat-xxxxxxxxxxxxxxxxxxxx`

3. Click **Add secret**

### Step 3: Verify Secret Creation
You should now see `GITLAB_TOKEN` listed under "Repository secrets". The value will be hidden for security.

---

## Using the Token in GitHub Actions

Here's an example workflow that uses the token to push code to GitLab:

```yaml
name: Sync to GitLab

on:
  push:
    branches:
      - main

jobs:
  sync:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Push to GitLab
        run: |
          git remote add gitlab https://oauth2:${{ secrets.GITLAB_TOKEN }}@gitlab.com/your-username/your-repo.git
          git push -f gitlab main:dev
```

**Remember to replace:**
- `your-username` with your GitLab username
- `your-repo` with your GitLab repository name
- `main:dev` with your desired branch mapping

---

## Updating an Expired Token

When your GitLab token expires, or if you need to generate a new token due to security concerns, follow these steps to renew it.

### When to Generate a New Token

- Token has expired or is approaching expiration
- You suspect the token has been compromised or exposed
- You need to change token permissions/scopes
- You're implementing security best practices (regular rotation)
- You've lost access to the original token
- You're migrating to a new authentication method

### Step 1: Revoke Old Token (Optional but Recommended)
1. Go to GitLab → Settings → Access Tokens
2. Find your old token in the "Active personal access tokens" list
3. Click **Revoke** next to the expired/old token

### Step 2: Create New Token
Follow the same steps as [Creating a GitLab Access Token](#creating-a-gitlab-access-token):
1. Navigate to GitLab → Settings → Access Tokens
2. Click **Add new token**
3. Use the same token name and scopes as before
4. Set a new expiration date
5. Click **Create personal access token**
6. Copy the new token immediately

### Step 3: Update GitHub Secret
1. Go to your GitHub repository → Settings → Secrets and variables → Actions
2. Find the `GITLAB_TOKEN` secret
3. Click on **GITLAB_TOKEN**
4. Click **Update secret**
5. Paste your new GitLab token
6. Click **Update secret**

### Step 4: Test the Update
1. Make a small commit to your repository:
   ```bash
   git commit --allow-empty -m "Test token update"
   git push origin main
   ```

2. Go to the **Actions** tab in GitHub
3. Watch your workflow run
4. Verify it completes successfully

✅ **Done!** Your workflow will now use the new token automatically.

---

## Renewing SSH Keys for GitLab

SSH keys are used when you push code directly from your local machine to GitLab. If you've received a notification that your SSH keys are expiring, follow these steps to renew them.

### What Are SSH Keys Used For?

SSH keys authenticate your local machine with GitLab when you use commands like:
```bash
git push origin main
git pull origin main
```

**Check if you're using SSH:** Run this command in your project directory:
```bash
git remote -v
```

If you see URLs like `git@gitlab.com:username/repo.git`, you're using SSH.

### Step 1: Generate a New SSH Key Pair

Follow the detailed instructions in the [SSH Key Generation Documentation](./ssh-setup.md) to create a new SSH key pair.

After generating your new key pair, you'll have:
- **Private key**: `~/.ssh/id_ed25519` (or similar)
- **Public key**: `~/.ssh/id_ed25519.pub` (or similar)

### Step 2: Add the Public Key to GitLab

1. Copy your public key to clipboard:
   ```bash
   # macOS
   pbcopy < ~/.ssh/id_ed25519.pub
   
   # Linux
   cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard
   
   # Windows (Git Bash)
   cat ~/.ssh/id_ed25519.pub | clip
   
   # Or manually
   cat ~/.ssh/id_ed25519.pub
   ```

2. Log in to [GitLab.com](https://gitlab.com)

3. Click on your **avatar** in the top-right corner

4. Select **Settings** (or **Preferences**)

5. In the left sidebar, click on **SSH Keys**

6. Click **Add new key**

7. Configure your SSH key:
   - **Key**: Paste your public key
   - **Title**: Give it a descriptive name
     - Example: `MacBook Pro 2024` or `Work Laptop`
   - **Usage type**: Select **Authentication & Signing** (or just **Authentication**)
   - **Expiration date**: Set an expiration date
     - Recommended: 1 year from now

8. Click **Add key**

### Step 3: Remove Old SSH Key (Optional but Recommended)

1. In GitLab → Settings → SSH Keys
2. Scroll down to see your existing keys
3. Find your old/expired key
4. Click the **Remove** button next to it
5. Confirm the removal

### Step 4: Test Your New SSH Key

Verify that your new SSH key works:

```bash
ssh -T git@gitlab.com
```

You should see a message like:
```
Welcome to GitLab, @your-username!
```

If you get an error, see the [SSH Troubleshooting](#ssh-troubleshooting) section below.

### Step 5: Update SSH Config (If Needed)

If you have multiple SSH keys or need to specify which key to use:

1. Edit your SSH config file:
   ```bash
   nano ~/.ssh/config
   ```

2. Add or update the GitLab configuration:
   ```
   Host gitlab.com
     HostName gitlab.com
     User git
     IdentityFile ~/.ssh/id_ed25519
     IdentitiesOnly yes
   ```

3. Save and exit

### SSH Troubleshooting

#### Permission Denied (publickey)
**Symptoms:** Can't push/pull, getting "Permission denied" error

**Solutions:**
- Verify you added the public key (`.pub` file), not the private key
- Check that the key was added successfully in GitLab Settings → SSH Keys
- Ensure your SSH agent is running: `eval "$(ssh-agent -s)"`
- Add your key to the agent: `ssh-add ~/.ssh/id_ed25519`
- Test the connection: `ssh -T git@gitlab.com`

#### Wrong Key Being Used
**Symptoms:** SSH connects but uses wrong identity

**Solutions:**
- Check which keys are loaded: `ssh-add -l`
- Remove all keys from agent: `ssh-add -D`
- Add only the correct key: `ssh-add ~/.ssh/id_ed25519`
- Use SSH config file to specify the correct key (see Step 5 above)

#### Key Not Recognized After Adding
**Symptoms:** Key added to GitLab but still getting authentication errors

**Solutions:**
- Wait 1-2 minutes for changes to propagate
- Clear SSH known hosts: `ssh-keygen -R gitlab.com`
- Verify key fingerprint matches in GitLab
- Check key permissions: `chmod 600 ~/.ssh/id_ed25519`

---

## Troubleshooting

### Authentication Failed
**Symptoms:** Workflow fails with "Authentication failed" or "Invalid credentials"

**Solutions:**
- Verify the token hasn't expired in GitLab
- Check that you copied the entire token (they're long!)
- Ensure the token has `write_repository` scope
- Confirm the GitHub secret was updated correctly

### Permission Denied
**Symptoms:** Workflow fails with "Permission denied" or "Access denied"

**Solutions:**
- Verify you have write access to the GitLab repository
- Check that the token has the correct scopes
- Ensure the GitLab repository URL in your workflow is correct

### Token Not Working Immediately
**Symptoms:** New token fails even though it was just created

**Solutions:**
- Wait 1-2 minutes for token to propagate through GitLab's systems
- Verify the secret name in GitHub matches what your workflow uses
- Check for any typos in the secret value (no extra spaces)

### Workflow Can't Find Secret
**Symptoms:** Workflow fails with "Secret not found" or undefined variable

**Solutions:**
- Verify the secret name matches exactly (case-sensitive)
- Ensure the secret is created at the repository level (not organization level)
- Check that you're looking at the correct repository

---

## Security Best Practices

### ✅ Do:
- Set expiration dates on all tokens
- Use the minimum required scopes
- Store tokens in a password manager
- Rotate tokens regularly (annually recommended)
- Revoke old tokens after creating new ones
- Set calendar reminders before expiration

### ❌ Don't:
- Share tokens with others
- Commit tokens to repositories
- Use overly broad scopes
- Leave tokens without expiration dates
- Store tokens in plain text
- Reuse tokens across multiple services

---

## Quick Reference Card

| Action | Location |
|--------|----------|
| Create GitLab Token | GitLab → Avatar → Settings → Access Tokens |
| Manage GitHub Secrets | GitHub Repo → Settings → Secrets and variables → Actions |
| Add SSH Key to GitLab | GitLab → Avatar → Settings → SSH Keys |
| Required Token Scope | `write_repository` |
| Token Format | `glpat-xxxxxxxxxxxxxxxxxxxx` |
| Recommended Expiration | 1 year (both tokens and SSH keys) |
| Secret Name | `GITLAB_TOKEN` (or custom) |

---

## Additional Resources

- [GitLab Personal Access Tokens Documentation](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)
- [GitHub Actions Secrets Documentation](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [GitHub Actions Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

---
