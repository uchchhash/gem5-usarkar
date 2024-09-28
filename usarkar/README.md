## Step 1: Check for Existing SSH Keys
1. Open Terminal (or Git Bash on Windows).
2. Check for existing SSH keys:
   ```bash
   ls -al ~/.ssh
   ```
   Look for files like `id_rsa` and `id_rsa.pub` or `id_ed25519` and `id_ed25519.pub`.

## Step 2: Generate a New SSH Key
If you don’t have SSH keys, generate a new one:
1. Run the following command:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
   (Use `rsa` if your system doesn’t support `ed25519`).
2. Press Enter to accept the default file location.

## Step 3: Add Your SSH Key to the SSH Agent
1. Start the SSH agent:
   ```bash
   eval "$(ssh-agent -s)"
   ```
2. Add your SSH key:
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

## Step 4: Copy Your Public Key
Copy your public key to your clipboard:
```bash
cat ~/.ssh/id_ed25519.pub
```

## Step 5: Add Your SSH Key to GitHub
1. Log in to GitHub.
2. Go to **Settings** > **SSH and GPG keys**.
3. Click **New SSH key**, paste your public key, and click **Add SSH key**.

## Step 6: Test Your SSH Connection
Test the SSH connection to GitHub:
```bash
ssh -T git@github.com
```
You should see a message confirming successful authentication.

## Step 7: Change Remote URL to Use SSH
If you cloned a repository using HTTPS, change the remote URL:
1. Navigate to your project directory:
   ```bash
   cd ~/path/to/your/repo
   ```
2. Set the remote URL to use SSH:
   ```bash
   git remote set-url origin git@github.com:your-username/repo-name.git
   ```

## Step 8: Push Your Changes
Push your branch to GitHub:
```bash
git push -u origin your-branch-name
```
