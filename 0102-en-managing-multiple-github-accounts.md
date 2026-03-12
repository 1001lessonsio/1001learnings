---
number: 102
lang: en
title: "How to: Best Practices for Working with Multiple GitHub Accounts"
date: 2026-02-28
stability: stable
review_by: 2027-02-28
author: "Stefan Wendel"
coauthor: "Claude Sonnet 4.6"
coauthor_id: claude-sonnet-4-6
---

# How to: Best Practices for Working with Multiple GitHub Accounts

**One SSH key per account. One alias per host. No manual switching.**

2026-02-28 · Stefan Wendel · Claude Sonnet 4.6

---

Every developer who uses multiple GitHub accounts — a personal one, one for their employer, one for a side project — knows the problem: Git doesn't ask which account you mean. It just picks the first matching key. The result: pushes under the wrong account, rejected authentications, or the classic blunder of landing a private commit under your company account.

The solution is straightforward and permanent: **one SSH key per account, configured via host aliases in `~/.ssh/config`.**

## The Setup

### 1. Generate SSH keys

Create a dedicated Ed25519 key for each account:

```bash
ssh-keygen -t ed25519 -C "you@example.com"  -f ~/.ssh/id_ed25519_personal
ssh-keygen -t ed25519 -C "you@company.com"  -f ~/.ssh/id_ed25519_work
ssh-keygen -t ed25519 -C "you@project.com"  -f ~/.ssh/id_ed25519_project
```

The `-f` flag sets the filename — all keys end up cleanly separated in `~/.ssh/`.

### 2. Add public keys to GitHub

For each account:
1. Log in to GitHub
2. **Settings → SSH and GPG keys → New SSH key**
3. Paste the contents of the respective `.pub` file

```bash
cat ~/.ssh/id_ed25519_personal.pub   # for account 1
cat ~/.ssh/id_ed25519_work.pub       # for account 2
cat ~/.ssh/id_ed25519_project.pub    # for account 3
```

### 3. Create ~/.ssh/config

```
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal

Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work

Host github-project
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_project
```

Each `Host` entry is an alias for `github.com` — with a fixed key assigned to it.

### 4. Test the connections

```bash
ssh -T git@github-personal
# Hi username-personal! You've successfully authenticated...

ssh -T git@github-work
# Hi username-work! You've successfully authenticated...
```

## Connecting repos correctly

Instead of `github.com` in the remote URL, just use the alias:

```bash
# New repo:
git remote add origin git@github-work:company/repo.git

# Switch an existing remote:
git remote set-url origin git@github-personal:username/repo.git
```

SSH looks up the alias in the config on connect, finds the matching entry, and automatically picks the right key — no further action needed.

## Why this works

`~/.ssh/config` is a routing table for SSH connections. When Git connects to `git@github-work:...`, SSH matches the host `github-work`, reads the assigned `IdentityFile`, and authenticates with exactly that key — regardless of what other keys exist on the system.

The result: **every repo pushes with the correct GitHub account automatically**, as long as the remote URL uses the right alias. No `ssh-add`, no environment switching, no risk of committing under the wrong account.

## The Bottom Line

Multiple GitHub accounts aren't an edge case — they're the normal situation for anyone who develops professionally. The key is to encode the separation once, cleanly, in `~/.ssh/config`. After that, everything runs on autopilot.

> **Configure once. Never think about it again.**

---

*Do you use multiple GitHub accounts? What pitfalls have you run into?*
