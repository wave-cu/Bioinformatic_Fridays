# Lesson 1 — Account Setup and Git Configuration

## Learning Objectives
- Create a free GitHub account and understand what GitHub is.
- Distinguish between Git (the tool) and GitHub (the platform).
- Install Git on Linux, macOS, or WSL.
- Configure Git with your name, email, and preferred editor.
- Generate an SSH key and add it to GitHub for passwordless authentication.
- Verify your setup end-to-end before writing a single line of code.

## Conceptual Overview

### Git vs GitHub
Git and GitHub are often mentioned together but they are different things:

- **Git** is a version control system. It runs entirely on your local machine and tracks changes to files over time. Think of it as a detailed logbook for your code.
- **GitHub** is a cloud platform that hosts Git repositories. It adds a web interface, collaboration features, issue tracking, and a place to back up your work.

You can use Git without GitHub, but GitHub without Git makes no sense. This module teaches both together because that is how they are used in practice.

### Why version control matters in bioinformatics
Bioinformatics workflows consist of scripts, configuration files, and notes. Without version control:
- You cannot easily tell which version of a script produced a particular result.
- Sharing a pipeline with a collaborator involves emailing files or copying folders with names like `analysis_v3_FINAL_revised2.sh`.
- Mistakes are hard to undo.

With Git, every change is recorded with a message, a timestamp, and the author's name. You can always return to any previous state.

### WSL note
If you are on Windows and running commands inside WSL, install Git inside WSL (not on Windows). The two environments share files but have separate software installations. Run all Git commands from inside the WSL terminal.

---

## Worked Examples

### 1) Create a GitHub account
Open a browser and go to `https://github.com`. Click **Sign up** and follow the steps.

Tips:
- Use an email address you check regularly — GitHub sends notifications there.
- Choose a username that is professional. Many employers look at GitHub profiles.
- The free plan is sufficient for everything in this module.

After signing up, verify your email address when GitHub sends the confirmation message.

---

### 2) Check whether Git is already installed

```bash
git --version
```

Expected output (version numbers will vary):
```
git version 2.43.0
```

If you see `command not found`, proceed to Example 3. If you see a version number, skip to Example 4.

---

### 3) Install Git

**On Ubuntu / Debian / WSL (Ubuntu):**
```bash
sudo apt update && sudo apt install git -y
```

Expected output (last few lines):
```
Setting up git (1:2.43.0-1ubuntu7) ...
Processing triggers for man-db ...
```

**On macOS (using Homebrew):**
```bash
brew install git
```

**On macOS (without Homebrew):**
Running `git --version` on macOS will prompt you to install the Xcode Command Line Tools if Git is not present. Follow the on-screen prompt.

After installation, confirm it worked:
```bash
git --version
```

Output:
```
git version 2.43.0
```

---

### 4) Configure your identity

Git records your name and email with every commit. Set them once and they apply to all repositories on your machine.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

No output is expected when these commands succeed.

Replace `"Your Name"` with your real name and `"you@example.com"` with the email you used to sign up on GitHub. These values will appear in your commit history and on GitHub.

---

### 5) Set your default editor

When Git opens a text editor (for example, to write a commit message), it uses the editor stored in `core.editor`. The default is `vi`, which can be confusing for beginners.

Set it to `nano`, which is simpler:
```bash
git config --global core.editor "nano"
```

Or, if you prefer Visual Studio Code and it is installed:
```bash
git config --global core.editor "code --wait"
```

No output is expected when these commands succeed.

---

### 6) Set the default branch name to `main`

Older versions of Git default to naming the first branch `master`. GitHub now uses `main`. Make them agree:

```bash
git config --global init.defaultBranch main
```

No output is expected when this command succeeds.

---

### 7) Review your configuration

`git config --list` prints every setting Git knows about.

```bash
git config --list
```

Output (yours will differ slightly):
```
user.name=Your Name
user.email=you@example.com
core.editor=nano
init.defaultbranch=main
```

---

### 8) Generate an SSH key

An SSH key lets you push to GitHub without typing your password every time. It works as a pair: a private key that stays on your machine, and a public key that you give to GitHub.

Check whether you already have a key:
```bash
ls ~/.ssh
```

If you see files named `id_ed25519` and `id_ed25519.pub`, you already have a key — skip to Example 9.

Generate a new key (replace the email with yours):
```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

You will be prompted three times:
```
Enter file in which to save the key (/home/youruser/.ssh/id_ed25519):
```
Press **Enter** to accept the default path.

```
Enter passphrase (empty for no passphrase):
```
Press **Enter** to leave it empty for now (acceptable for a learning setup).

```
Enter same passphrase again:
```
Press **Enter** again.

Final output:
```
Your identification has been saved in /home/youruser/.ssh/id_ed25519
Your public key has been saved in /home/youruser/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx you@example.com
The key's randomart image is:
+--[ED25519 256]--+
...
+----[SHA256]-----+
```

---

### 9) Copy your public key

Print the public key so you can paste it into GitHub:
```bash
cat ~/.ssh/id_ed25519.pub
```

Output (your key will be different):
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAbcDeFgHiJkLmNoPqRsTuVwXyZ0123456789abcdef you@example.com
```

Select the entire output line (starting with `ssh-ed25519`) and copy it to your clipboard.

---

### 10) Add the public key to GitHub

1. Go to `https://github.com/settings/keys` in your browser.
2. Click **New SSH key**.
3. In the **Title** field, enter a name for this machine (e.g. `WSL Laptop`).
4. In the **Key** field, paste the line you copied in Example 9.
5. Click **Add SSH key**.

---

### 11) Test the SSH connection

```bash
ssh -T git@github.com
```

First-time output (type `yes` and press Enter when asked):
```
The authenticity of host 'github.com (140.82.121.3)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

After you type `yes`:
```
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi YourUsername! You've successfully authenticated, but GitHub does not provide shell access.
```

If you see your username in that message, authentication is working correctly.

---

## Exercises

1) Run `git --version`. What version is installed on your machine?

2) Run `git config --list`. Confirm that `user.name`, `user.email`, and `init.defaultbranch` are all set correctly.

3) Run `cat ~/.ssh/id_ed25519.pub`. Does the output start with `ssh-ed25519`? If the file does not exist, generate a key using Example 8.

4) Run `ssh -T git@github.com`. Do you see your GitHub username in the response?

5) Challenge: Go to `https://github.com/settings/profile` and add a short bio and your institution. This makes your profile more useful to collaborators.

---

## Solutions

### Solution 1
```bash
git --version
```
Output (version will vary):
```
git version 2.43.0
```

### Solution 2
```bash
git config --list
```
Look for these three lines in the output:
```
user.name=Your Name
user.email=you@example.com
init.defaultbranch=main
```

### Solution 3
```bash
cat ~/.ssh/id_ed25519.pub
```
The line should start with `ssh-ed25519`. If the file is missing, run:
```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

### Solution 4
```bash
ssh -T git@github.com
```
Expected output:
```
Hi YourUsername! You've successfully authenticated, but GitHub does not provide shell access.
```

### Solution 5
Navigate to `https://github.com/settings/profile`, fill in the **Bio** and **Company** fields, and click **Update profile**.
