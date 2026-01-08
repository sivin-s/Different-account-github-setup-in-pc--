Understood.
Below is a **clean `README.md`**, recreated **without extra comment labels like `# Account 3`**, exactly in the format you want.

You can copy-paste this **directly as `README.md`**.

---

# Multiple GitHub Accounts Using SSH

Manage multiple GitHub accounts on the same machine **without recreating SSH keys**.

---

## Problem

* SSH uses one default key
* GitHub authenticates only one account
* Recreating SSH keys repeatedly is inefficient

---

## Solution

* One SSH key per GitHub account
* Use SSH host aliases
* Bind each repository to a specific account

---

## Prerequisites

* Git installed
* SSH installed
* Multiple GitHub accounts

---

## Step 1: Generate SSH Keys (One-Time)

```bash
ssh-keygen -t ed25519 -C "account1@email.com" -f ~/.ssh/id_github_account1
ssh-keygen -t ed25519 -C "account2@email.com" -f ~/.ssh/id_github_account2
ssh-keygen -t ed25519 -C "account3@email.com" -f ~/.ssh/id_github_account3
ssh-keygen -t ed25519 -C "account4@email.com" -f ~/.ssh/id_github_account4
```

Do not overwrite existing keys.

---

## Step 2: Add Keys to SSH Agent

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add all keys:

```bash
ssh-add ~/.ssh/id_github_account1
ssh-add ~/.ssh/id_github_account2
ssh-add ~/.ssh/id_github_account3
ssh-add ~/.ssh/id_github_account4
```

---

## Step 3: Add Public Keys to GitHub

Copy each public key:

```bash
cat ~/.ssh/id_github_account1.pub
```

Add it in GitHub:

```
GitHub → Settings → SSH and GPG keys → New SSH key
```

Repeat for all accounts.

---

## Step 4: Configure SSH (`~/.ssh/config`)

Edit or create the SSH config file:

```bash
nano ~/.ssh/config
```

Add the following:

```ini
Host github-account1
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_github_account1

Host github-account2
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_github_account2

Host github-account3
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_github_account3

Host github-account4
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_github_account4
```

---

## Step 5: Clone Repositories Using Correct Account

Do **not** use:

```bash
git@github.com:username/repo.git
```

Use the SSH alias:

```bash
git clone git@github-account1:username/repo.git
git clone git@github-account2:company/project.git
```

---

## Step 6: Fix Existing Repository Remote

If a repository was cloned with the wrong account:

```bash
git remote set-url origin git@github-account1:username/repo.git
git push
```

---

## Step 7: Verify SSH Authentication

```bash
ssh -T git@github-account1
```

Expected output:

```
Hi username! You've successfully authenticated.
```

---

## Optional: Set Git User Per Repository

```bash
git config user.name "Your Name"
git config user.email "your@email.com"
```

---

## Summary

* One SSH key per GitHub account
* SSH host aliases control authentication
* No need to recreate SSH keys
* Works with unlimited GitHub accounts

---

## License

MIT

---

If you want next:

* Windows version
* Fix commits pushed with wrong author
* Org + personal GitHub workflow
