# Lesson 3 — Pushing to GitHub

## Learning Objectives
- Create a new repository on GitHub through the web interface.
- Connect a local repository to a GitHub remote with `git remote add`.
- Upload commits to GitHub with `git push`.
- Download updates from GitHub with `git pull`.
- Clone an existing GitHub repository with `git clone`.
- Understand the difference between `origin`, `main`, and `HEAD`.

## Conceptual Overview

### What is a remote?
A remote is a version of your repository that lives somewhere else — in this case, on GitHub's servers. You can have several remotes, but the most common one is called **origin** by convention. When you push, you are sending your local commits to the remote. When you pull, you are fetching commits from the remote and merging them into your local branch.

### The push/pull cycle
```
Local machine                   GitHub (origin)
─────────────                   ───────────────
edit files
git add
git commit   ──── git push ──►  repository
             ◄─── git pull ───  (updated by a collaborator)
```

### HTTPS vs SSH
GitHub supports two ways to authenticate when pushing:
- **HTTPS** — you use your GitHub username and a Personal Access Token (PAT) as the password.
- **SSH** — you use the SSH key you set up in Lesson 1. No password needed after setup.

This module uses SSH throughout because it is more convenient once configured.

---

## Worked Examples

### 1) Create a repository on GitHub

1. Go to `https://github.com/new` in your browser.
2. Fill in the fields:
   - **Repository name:** `my_analysis` (match the folder name from Lesson 2)
   - **Description:** `Bioinformatics analysis scripts and notes`
   - **Visibility:** Public (or Private — both work the same way)
   - **Do NOT tick** "Add a README file", "Add .gitignore", or "Choose a license" — you already have these locally.
3. Click **Create repository**.

GitHub will show a page titled "Quick setup". Leave this page open — you will need the SSH URL in the next step.

---

### 2) Copy the SSH remote URL

On the "Quick setup" page, make sure **SSH** is selected (not HTTPS). The URL looks like:

```
git@github.com:YourUsername/my_analysis.git
```

Copy this URL.

---

### 3) Add the remote to your local repository

Go back to your terminal and navigate to your project:

```bash
cd ~/projects/my_analysis
```

Add the remote. `origin` is the name you give to this remote — it is a universal convention:

```bash
git remote add origin git@github.com:YourUsername/my_analysis.git
```

No output is expected when this command succeeds.

Verify it was added:
```bash
git remote -v
```

Output:
```
origin	git@github.com:YourUsername/my_analysis.git (fetch)
origin	git@github.com:YourUsername/my_analysis.git (push)
```

Two lines appear: one for fetching (downloading) and one for pushing (uploading). Both point to the same URL.

---

### 4) Push your commits to GitHub

`git push -u origin main` uploads your local `main` branch to the remote named `origin`. The `-u` flag links the local branch to the remote branch — after this first push you only need to type `git push` in future.

```bash
git push -u origin main
```

Output:
```
Enumerating objects: 14, done.
Counting objects: 100% (14/14), done.
Delta compression using up to 8 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (14/14), 1.23 KiB | 1.23 MiB/s, done.
Total 14 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), done.
To git@github.com:YourUsername/my_analysis.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Go to `https://github.com/YourUsername/my_analysis` in your browser. Your files are now online.

---

### 5) Make a change locally and push it

Edit `notes.md`:

```bash
nano notes.md
```

Add a new line at the bottom:
```
- Pushed repository to GitHub successfully
```

Save and exit. Stage, commit, and push:

```bash
git add notes.md
git commit -m "Note successful GitHub push"
git push
```

Output:
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 320 bytes | 320.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
To git@github.com:YourUsername/my_analysis.git
   f4a52b1..8e12ab3  main -> main
```

Refresh the GitHub page for your repository. The commit message appears in the file list.

---

### 6) View commits on GitHub

On your repository page, click the clock icon that shows the number of commits (e.g. "7 commits") near the top right of the file list. GitHub shows the full commit history with messages, authors, and timestamps — the same information as `git log`.

---

### 7) Edit a file directly on GitHub

GitHub allows you to edit files in the browser. This is useful for small changes like fixing a typo in a README.

1. Go to your repository page on GitHub.
2. Click on `README.md`.
3. Click the pencil icon (Edit this file) in the top right.
4. Add a line at the bottom: `Maintained by: Your Name`
5. Scroll down to the "Commit changes" section.
6. Enter the message: `Add maintainer name to README`
7. Click **Commit changes**.

The change now exists on GitHub but not yet on your local machine.

---

### 8) Pull the change to your local machine

`git pull` fetches the latest commits from the remote and merges them into your current branch.

```bash
git pull
```

Output:
```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
Unpacking objects: 100% (3/3), done.
From github.com:YourUsername/my_analysis
   8e12ab3..9f23bc4  main       -> origin/main
Updating 8e12ab3..9f23bc4
Fast-forward
 README.md | 1 +
 1 file changed, 1 insertion(+)
```

Confirm the change is now in your local file:
```bash
tail -n 3 README.md
```

Output:
```
Samples: SRR1553607, SRR1553608, SRR1553609
Maintained by: Your Name
```

---

### 9) Clone an existing repository

`git clone` downloads a complete copy of a remote repository (all files and full history) to your local machine. This is how you start working on a repository that already exists on GitHub — for example, a collaborator's project or a public tool.

Clone the Bioinformatic Fridays repository as an example (read-only, no login needed):

```bash
cd ~
git clone git@github.com:wave-cu/Bioinformatic_Fridays.git
```

Output:
```
Cloning into 'Bioinformatic_Fridays'...
remote: Enumerating objects: 45, done.
remote: Counting objects: 100% (45/45), done.
remote: Compressing objects: 100% (38/38), done.
Receiving objects: 100% (45/45), 25.14 KiB | 1.40 MiB/s, done.
```

Explore the cloned repository:
```bash
ls Bioinformatic_Fridays
```

Output:
```
Module_1_Linux  Module_2_Conda  Module_3_Omics  Module_4_Recap_Exercises  Module_5_Project  Module_6_Problem_Solving  Module_7_Bash_Scripting  README.md
```

The entire repository, including all history, is now on your machine.

---

### 10) Understand `git status` after a push

After pushing, your working directory and remote are in sync. Run:

```bash
cd ~/projects/my_analysis
git status
```

Output:
```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

"Your branch is up to date with 'origin/main'" means your local branch matches what is on GitHub exactly.

---

## Exercises

1) Go to `https://github.com/YourUsername/my_analysis`. Can you see all the files from Lesson 2? Click on `run_fastqc.sh` and read its contents.

2) Create a new file called `methods.md` locally with a short description of your analysis approach. Stage, commit, and push it. Refresh GitHub to confirm it appears.

3) Use the GitHub web editor to add a line to `sample_list.txt` (add `SRR1553610`). Then run `git pull` locally and confirm the new line is present.

4) Run `git log --oneline` after pulling. How does the history compare to what you see on the GitHub commits page?

5) Challenge: Clone any public GitHub repository you are curious about. Run `git log --oneline` inside it. How many commits does it have?

---

## Solutions

### Solution 1
Go to `https://github.com/YourUsername/my_analysis`. Files listed: `README.md`, `notes.md`, `sample_list.txt`, `run_fastqc.sh`, `environment.md`, `.gitignore`.

### Solution 2
```bash
cd ~/projects/my_analysis
nano methods.md
```
File content:
```
## Methods

1. Quality control with FastQC
2. Adapter trimming with Trimmomatic
3. Alignment to reference genome with BWA-MEM
4. Variant calling with GATK HaplotypeCaller
```

```bash
git add methods.md
git commit -m "Add analysis methods overview"
git push
```

### Solution 3
On GitHub: navigate to `sample_list.txt`, click the pencil icon, add `SRR1553610` on a new line, commit the change.

Back in the terminal:
```bash
git pull
cat sample_list.txt
```
Output:
```
SRR1553607
SRR1553608
SRR1553609
SRR1553610
```

### Solution 4
```bash
git log --oneline
```
The list should match the commits shown on the GitHub commits page exactly, in the same order.

### Solution 5
```bash
git clone git@github.com:wave-cu/Bioinformatic_Fridays.git ~/bfridays_clone
cd ~/bfridays_clone
git log --oneline | wc -l
```
The number printed is the total commit count.
