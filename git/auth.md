# GitHub Authentication: Stop Typing Passwords!

If you are tired of entering your username and Personal Access Token (PAT) every time you clone, push, or pull from a GitHub repository, you can automate the process. 

This short tutorial covers two best ways: **SSH Keys** (Recommended) and **HTTPS Credential Caching**.

---

## Method 1: Use SSH Keys (Recommended)
SSH uses a pair of cryptographic keys (public and private) to securely identify your computer. Once set up, you will enjoy a completely passwordless experience.

### Step 1: Generate a New SSH Key
Open your terminal (or Git Bash on Windows) and run the following command. Replace the email with your GitHub email address:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
> **Tip:** Press **Enter** to accept the default file location. When asked for a passphrase, you can leave it empty and press Enter twice for a seamless experience.

### Step 2: Copy Your Public Key
Copy the output of your newly generated public key. Use the command for your specific Operating System:
* **Mac:** `pbcopy < ~/.ssh/id_ed25519.pub`
* **Windows:** `clip < ~/.ssh/id_ed25519.pub`
* **Linux:** `cat ~/.ssh/id_ed25519.pub` *(then manually copy the printed text)*

### Step 3: Add the Key to GitHub
1. Log in to [GitHub](https://github.com) and go to **Settings** (click your profile picture in the top right).
2. Navigate to **SSH and GPG keys** in the left sidebar.
3. Click the green **New SSH key** button.
4. Give it a descriptive title (e.g., "My Work Laptop") and paste your copied key into the "Key" field.
5. Click **Add SSH key**.

### Step 4: Clone Using SSH
From now on, copy the **SSH** URL from GitHub instead of the HTTPS one:
```bash
git clone git@github.com:username/repository.git
```

---

## Method 2: Cache your HTTPS Credentials
If you prefer using HTTPS links, you can tell Git to remember your Personal Access Token (PAT) so you only have to enter it once.

### Step 1: Turn on the Credential Helper
Run the appropriate command for your Operating System:

**For Mac (Uses Keychain):**
```bash
git config --global credential.helper osxkeychain
```

**For Windows (Uses Git Credential Manager):**
```bash
git config --global credential.helper manager
```

**For Linux (Stores in a local file):**
```bash
git config --global credential.helper store
```
*(Note: Using `store` saves your credentials in plain text in a `.git-credentials` file. For better security on Linux, look into `libsecret`).*

### Step 2: Clone the Repository
Clone your repository using the standard HTTPS link:
```bash
git clone https://github.com/username/repository.git
```
Git will prompt you for your username and your Personal Access Token. Once entered successfully, your OS will securely save them. You won't be asked again for future clones!
