# Lesson 2 — Your First Repository

## Learning Objectives
- Understand what a Git repository is and what the `.git` folder contains.
- Initialise a new repository with `git init`.
- Stage files with `git add` and understand the staging area.
- Record a snapshot with `git commit` and write a good commit message.
- Check repository status with `git status` and `git log`.
- Create a `.gitignore` file to exclude files Git should not track.

## Conceptual Overview

### What is a repository?
A Git repository (or "repo") is a folder that Git is tracking. When you run `git init` inside a folder, Git creates a hidden subfolder called `.git`. That folder holds the entire history of every file in the project — every version you have ever committed. You should never edit files inside `.git` directly.

### The three areas
Understanding these three areas is the key to understanding Git:

| Area | What it is |
|---|---|
| **Working directory** | The files as they exist on your filesystem right now |
| **Staging area (index)** | A preparation zone — files you have marked as "ready to commit" |
| **Repository (history)** | The permanent record of all commits |

The workflow is always the same:
1. Edit files in your working directory.
2. Stage the changes you want to record with `git add`.
3. Commit the staged changes with `git commit`.

### What makes a good commit message?
A commit message should complete the sentence "If applied, this commit will…"

- Good: `Add quality-control script for FASTQ files`
- Good: `Fix off-by-one error in read counter`
- Too vague: `update` or `fix` or `changes`

Short messages (under 72 characters) are fine for most commits.

---

## Worked Examples

### 1) Create a project folder and initialise a repository

```bash
mkdir -p ~/projects/my_analysis
cd ~/projects/my_analysis
git init
```

Output:
```
Initialized empty Git repository in /home/youruser/projects/my_analysis/.git/
```

`git init` only needs to be run once per project. The `.git` folder is now present:
```bash
ls -a
```

Output:
```
.  ..  .git
```

---

### 2) Create your first file

```bash
nano README.md
```

Type the following inside nano, then press `Ctrl+O` to save and `Ctrl+X` to exit:
```
# My Analysis

This repository contains scripts and notes for my bioinformatics project.
```

Confirm the file exists:
```bash
ls
```

Output:
```
README.md
```

---

### 3) Check the repository status

`git status` tells you what Git sees in your working directory and staging area.

```bash
git status
```

Output:
```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

"Untracked files" means Git can see the file but is not recording changes to it yet.

---

### 4) Stage the file

`git add` moves a file from the working directory into the staging area.

```bash
git add README.md
```

No output is expected when this command succeeds. Check the status again:

```bash
git status
```

Output:
```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   README.md
```

The file is now in the staging area — it is ready to be committed.

---

### 5) Make your first commit

`git commit -m` records the staged changes with the message provided after `-m`.

```bash
git commit -m "Add project README"
```

Output:
```
[main (root-commit) a3f92c1] Add project README
 1 file changed, 3 insertions(+)
 create mode 100644 README.md
```

The hash (`a3f92c1`) will be different on your machine — it is unique to your commit.

---

### 6) View commit history

`git log` lists all commits from newest to oldest.

```bash
git log
```

Output:
```
commit a3f92c1d4e5f678901234567890abcdef1234567 (HEAD -> main)
Author: Your Name <you@example.com>
Date:   Thu Jun 26 09:00:00 2025 +0000

    Add project README
```

`git log --oneline` gives a compact one-line-per-commit view. This is useful when the history is long.

```bash
git log --oneline
```

Output:
```
a3f92c1 (HEAD -> main) Add project README
```

---

### 7) Make a second commit

Add a new file to practice the full workflow again:

```bash
nano notes.md
```

Type the following, then save and exit:
```
## Session notes

- Installed FastQC
- Downloaded SRR1553607 dataset
```

Stage and commit:

```bash
git add notes.md
git commit -m "Add session notes"
```

Output:
```
[main b7c10d2] Add session notes
 1 file changed, 4 insertions(+)
 create mode 100644 notes.md
```

View the updated history:

```bash
git log --oneline
```

Output:
```
b7c10d2 (HEAD -> main) Add session notes
a3f92c1 Add project README
```

---

### 8) Stage multiple files at once

Create two more files:

```bash
nano sample_list.txt
```

Type the following, then save and exit:
```
SRR1553607
SRR1553608
SRR1553609
```

```bash
nano run_fastqc.sh
```

Type the following, then save and exit:
```bash
#!/usr/bin/env bash
# Run FastQC on all FASTQ files in the current directory
fastqc *.fastq -o fastqc_results/
```

Stage both files in one command:
```bash
git add sample_list.txt run_fastqc.sh
```

Check status:
```bash
git status
```

Output:
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   run_fastqc.sh
	new file:   sample_list.txt
```

Commit:
```bash
git commit -m "Add sample list and FastQC script"
```

Output:
```
[main c1e20f3] Add sample list and FastQC script
 2 files changed, 5 insertions(+)
 create mode 100755 run_fastqc.sh
 create mode 100644 sample_list.txt
```

---

### 9) Create a `.gitignore` file

A `.gitignore` file tells Git which files and folders to ignore completely. This is important for:
- Large data files that do not belong in a repository (`.fastq`, `.bam`, `.vcf`)
- Temporary files created by tools (`*.log`, `*.tmp`)
- System files (`.DS_Store` on macOS)

```bash
nano .gitignore
```

Type the following, then save and exit:
```
# Large sequencing data files
*.fastq
*.fastq.gz
*.bam
*.bam.bai
*.vcf
*.vcf.gz

# Tool output directories
fastqc_results/
trimmed/
aligned/

# Temporary and system files
*.log
*.tmp
.DS_Store
__pycache__/
```

Stage and commit the `.gitignore`:
```bash
git add .gitignore
git commit -m "Add .gitignore for sequencing data and temp files"
```

Output:
```
[main d2f31a4] Add .gitignore for sequencing data and temp files
 1 file changed, 15 insertions(+)
 create mode 100644 .gitignore
```

Test it — create a dummy FASTQ file and check whether Git ignores it:
```bash
touch test_sample.fastq
git status
```

Output:
```
On branch main
nothing to commit, working tree clean
```

Git does not list `test_sample.fastq` because `.gitignore` excludes `*.fastq`. Remove the test file:
```bash
rm test_sample.fastq
```

---

### 10) See what changed in a file

Edit `notes.md` to add a line:
```bash
nano notes.md
```

Add a new line at the bottom:
```
- FastQC completed successfully on all samples
```

Save and exit. Now run `git diff` to see what changed before staging:

```bash
git diff notes.md
```

Output:
```
diff --git a/notes.md b/notes.md
index 3b4f2a1..e7c90d2 100644
--- a/notes.md
+++ b/notes.md
@@ -2,3 +2,4 @@

 - Installed FastQC
 - Downloaded SRR1553607 dataset
+- FastQC completed successfully on all samples
```

Lines starting with `+` are additions. Lines starting with `-` are deletions. Stage and commit the change:

```bash
git add notes.md
git commit -m "Update notes with FastQC results"
```

---

## Exercises

1) Run `git log --oneline` in `~/projects/my_analysis`. How many commits do you have?

2) Create a new file called `environment.md` that lists the tools you have installed (at least two). Stage and commit it with an appropriate message.

3) Edit `README.md` to add a second line describing your project. Run `git diff` before staging to confirm the change is shown. Then stage and commit.

4) Add `*.csv` and `*.tsv` to your `.gitignore`. Create a dummy file called `metadata.csv` and verify with `git status` that Git ignores it. Then remove the dummy file.

5) Challenge: Run `git log --oneline` again. Your history should now have at least 6 commits. Write down what each commit does in your own words.

---

## Solutions

### Solution 1
```bash
git log --oneline
```
Output (yours will show 4 commits from the worked examples):
```
d2f31a4 (HEAD -> main) Add .gitignore for sequencing data and temp files
c1e20f3 Add sample list and FastQC script
b7c10d2 Add session notes
a3f92c1 Add project README
```

### Solution 2
```bash
nano environment.md
```
File content:
```
## Software environment

- FastQC 0.12.1
- Trimmomatic 0.39
- BWA 0.7.17
```

```bash
git add environment.md
git commit -m "Add software environment notes"
```

### Solution 3
```bash
nano README.md
```
Add a second line, for example:
```
Samples: SRR1553607, SRR1553608, SRR1553609
```

```bash
git diff README.md
```
Output shows the new line with a `+` prefix.
```bash
git add README.md
git commit -m "Add sample list to README"
```

### Solution 4
Open `.gitignore` and add two lines:
```bash
nano .gitignore
```
Add at the bottom:
```
*.csv
*.tsv
```
Save and exit.
```bash
touch metadata.csv
git status
```
Output:
```
On branch main
nothing to commit, working tree clean
```
`metadata.csv` does not appear. Remove it:
```bash
rm metadata.csv
```

### Solution 5
```bash
git log --oneline
```
Output (6 or more commits depending on your exercises):
```
f4a52b1 (HEAD -> main) Add sample list to README
e3c41a0 Add software environment notes
d2f31a4 Add .gitignore for sequencing data and temp files
c1e20f3 Add sample list and FastQC script
b7c10d2 Add session notes
a3f92c1 Add project README
```
