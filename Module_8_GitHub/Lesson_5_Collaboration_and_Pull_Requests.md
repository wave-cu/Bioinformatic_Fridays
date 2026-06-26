# Lesson 5 — Collaboration and Pull Requests

## Learning Objectives
- Fork a public repository and understand what a fork is.
- Open a pull request (PR) to propose changes to someone else's repository.
- Review and comment on a pull request.
- Use GitHub Issues to report bugs and suggest improvements.
- Understand `git fetch` vs `git pull`.
- Keep your fork up to date with the original repository (upstream).

## Conceptual Overview

### The collaboration workflow
When you do not have write access to a repository (for example, a public project you want to contribute to), you follow this pattern:

1. **Fork** — create your own copy of the repository on GitHub.
2. **Clone** — download your fork to your local machine.
3. **Branch** — create a branch for your changes.
4. **Edit, commit, push** — make your changes and push to your fork.
5. **Pull request** — ask the original repository to accept your changes.

For repositories where you do have write access (your own repos, or team repos), you skip forking and work directly with branches.

### Pull requests
A pull request (PR) is a formal proposal to merge your branch into someone else's (or your own) repository. It includes:
- A list of all commits in your branch
- A diff showing every line changed
- A comment thread for discussion and review
- A merge button for the repository owner

Pull requests are the central unit of collaboration on GitHub.

### Issues
An Issue is a GitHub feature for tracking tasks, bugs, and feature requests. Issues are linked to a repository and can be assigned to people, labelled, and referenced from commits and pull requests.

---

## Worked Examples

### 1) Fork a repository on GitHub

Forking creates your own independent copy of a repository under your GitHub account.

1. Go to `https://github.com/wave-cu/Bioinformatic_Fridays` in your browser.
2. Click the **Fork** button in the top right corner.
3. On the fork page, leave the default settings and click **Create fork**.

GitHub redirects you to `https://github.com/YourUsername/Bioinformatic_Fridays` — this is your fork. It is a full copy of the original repository.

---

### 2) Clone your fork locally

```bash
cd ~
git clone git@github.com:YourUsername/Bioinformatic_Fridays.git
cd Bioinformatic_Fridays
```

Output:
```
Cloning into 'Bioinformatic_Fridays'...
remote: Enumerating objects: 45, done.
remote: Counting objects: 100% (45/45), done.
Receiving objects: 100% (45/45), 25.14 KiB | 2.00 MiB/s, done.
```

Check the remote:
```bash
git remote -v
```

Output:
```
origin	git@github.com:YourUsername/Bioinformatic_Fridays.git (fetch)
origin	git@github.com:YourUsername/Bioinformatic_Fridays.git (push)
```

---

### 3) Add the original repository as a second remote (upstream)

Keeping track of the original repository lets you pull in new changes from it later. By convention it is called **upstream**:

```bash
git remote add upstream git@github.com:wave-cu/Bioinformatic_Fridays.git
git remote -v
```

Output:
```
origin	git@github.com:YourUsername/Bioinformatic_Fridays.git (fetch)
origin	git@github.com:YourUsername/Bioinformatic_Fridays.git (push)
upstream	git@github.com:wave-cu/Bioinformatic_Fridays.git (fetch)
upstream	git@github.com:wave-cu/Bioinformatic_Fridays.git (push)
```

---

### 4) Create a branch for your contribution

Never make changes directly on `main` when preparing a pull request. Always use a branch:

```bash
git switch -c fix-typo-readme
```

Output:
```
Switched to a new branch 'fix-typo-readme'
```

---

### 5) Make a small change

Open the main README:
```bash
nano README.md
```

Add a line at the very bottom:
```
*This repository is actively maintained.*
```

Save and exit.

Stage and commit:
```bash
git add README.md
git commit -m "Add maintenance notice to README"
```

Output:
```
[fix-typo-readme 7f3a2b1] Add maintenance notice to README
 1 file changed, 1 insertion(+)
```

---

### 6) Push the branch to your fork

```bash
git push -u origin fix-typo-readme
```

Output:
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 335 bytes | 335.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
remote:
remote: Create a pull request for 'fix-typo-readme' on GitHub by visiting:
remote:      https://github.com/YourUsername/Bioinformatic_Fridays/pull/new/fix-typo-readme
remote:
To git@github.com:YourUsername/Bioinformatic_Fridays.git
 * [new branch]      fix-typo-readme -> fix-typo-readme
branch 'fix-typo-readme' set up to track 'origin/fix-typo-readme'.
```

GitHub even shows you the URL for creating the pull request directly.

---

### 7) Open a pull request on GitHub

1. Go to `https://github.com/YourUsername/Bioinformatic_Fridays` in your browser.
2. GitHub will show a banner: "fix-typo-readme had recent pushes — Compare & pull request". Click it.
   - If the banner is gone, click the **Pull requests** tab and then **New pull request**.
3. The "base" should be `wave-cu/Bioinformatic_Fridays` → `main`. The "compare" should be your fork → `fix-typo-readme`.
4. Fill in the pull request:
   - **Title:** `Add maintenance notice to README`
   - **Description:** Explain what you changed and why. For example:
     ```
     Added a short note to the README indicating the repository is actively maintained.
     This helps visitors know the project is current.
     ```
5. Click **Create pull request**.

The repository owner (wave-cu) will now see your pull request. They can comment, request changes, or merge it.

---

### 8) Explore a pull request

On any open pull request page, notice the tabs:
- **Conversation** — comments and review discussion
- **Commits** — every commit in this PR
- **Files changed** — a line-by-line diff of every change

Click **Files changed** on your own PR. Red lines are deletions, green lines are additions. This is the same information as `git diff` but formatted in the browser.

---

### 9) Open a GitHub Issue

Issues are used to report problems, ask questions, or propose features. They live in the repository at the **Issues** tab.

To open an issue on any repository:
1. Go to the repository page and click the **Issues** tab.
2. Click **New issue**.
3. Write a clear title and description. For a bug report, include:
   - What you expected to happen
   - What actually happened
   - The exact command you ran and the error output
4. Click **Submit new issue**.

You can also reference an issue from a commit message. If an issue is number 12, including `Closes #12` in a commit message will automatically close that issue when the commit is merged.

Example commit message:
```
Fix path error in FastQC script

Closes #12
```

---

### 10) Sync your fork with upstream

When the original repository gets new commits, your fork falls behind. Fetch the new commits from upstream and merge them into your local `main`:

```bash
git switch main
git fetch upstream
```

Output:
```
From github.com:wave-cu/Bioinformatic_Fridays
 * [new branch]      main       -> upstream/main
```

`git fetch` downloads new commits but does not change your files. The difference from `git pull` is:
- `git fetch` — download commits, do not merge
- `git pull` — download commits AND merge (equivalent to `git fetch` + `git merge`)

Merge the upstream changes into your local `main`:
```bash
git merge upstream/main
```

Output (if the original repo had new commits):
```
Updating f4a52b1..9d3e7c2
Fast-forward
 Module_1_Linux/README.md | 2 ++
 1 file changed, 2 insertions(+)
```

Push the updated `main` to your fork:
```bash
git push origin main
```

Your fork is now up to date with the original repository.

---

### 11) Use `git log` to see all contributors

In a cloned repository with multiple contributors, `git log --oneline --graph` shows branches and merges visually:

```bash
cd ~/Bioinformatic_Fridays
git log --oneline --graph
```

Output (example):
```
* 9d3e7c2 (HEAD -> main, origin/main) Update Module 3 with new omics dataset
* 7b2a1f0 Add Module 7 bash scripting
* 4c9d3e1 Fix typo in Module 1 lesson 2
* 1a8b2c3 Add Module 6 problem solving content
...
```

The `*` marks each commit, and lines connecting them show branch and merge history. In a repository with many contributors and branches, this view shows the full parallel development history.

---

## Exercises

1) Go to your fork of `Bioinformatic_Fridays` on GitHub. When was it last updated? Is it behind the original?

2) Create a new branch on your fork called `add-resources`. Add a file called `resources.md` with three links to useful bioinformatics resources (they can be tool names with brief descriptions). Push the branch and open a pull request.

3) Open a GitHub Issue on your own `my_analysis` repository (from Lessons 2–4). The issue title should be: "Add a GATK variant calling script". Add a short description of what the script should do.

4) Run `git fetch upstream` in your Bioinformatic_Fridays clone. What does `git log --oneline upstream/main` show?

5) Challenge: On your `my_analysis` repository, create a branch, make a commit, push it, and open a pull request from that branch into `main` — on your own repository. Then merge the pull request using the GitHub web interface and pull the merged changes locally.

---

## Solutions

### Solution 1
Go to `https://github.com/YourUsername/Bioinformatic_Fridays`. The top of the page shows "This branch is up to date with wave-cu:main" or "This branch is N commit(s) behind wave-cu:main".

### Solution 2
```bash
cd ~/Bioinformatic_Fridays
git switch -c add-resources
nano resources.md
```
File content:
```
## Bioinformatics Resources

- **FastQC** — Quality control for raw sequencing reads. https://www.bioinformatics.babraham.ac.uk/projects/fastqc/
- **GATK** — Genome Analysis Toolkit for variant calling. https://gatk.broadinstitute.org/
- **Bioconductor** — R packages for bioinformatics analysis. https://www.bioconductor.org/
```

```bash
git add resources.md
git commit -m "Add bioinformatics resources list"
git push -u origin add-resources
```
Then open a pull request on GitHub from `add-resources` into `main`.

### Solution 3
Go to `https://github.com/YourUsername/my_analysis/issues/new`.
- Title: `Add a GATK variant calling script`
- Description:
  ```
  We need a script that runs GATK HaplotypeCaller on aligned BAM files.
  It should accept a sample name as an argument and output a VCF file.
  ```
Click **Submit new issue**.

### Solution 4
```bash
git fetch upstream
git log --oneline upstream/main
```
This shows all commits on the original repository's `main` branch, regardless of what is in your fork.

### Solution 5
```bash
cd ~/projects/my_analysis
git switch -c add-gatk-script
nano run_gatk.sh
```
File content:
```bash
#!/usr/bin/env bash
SAMPLE=$1
gatk HaplotypeCaller \
    -R reference.fa \
    -I aligned/${SAMPLE}.bam \
    -O variants/${SAMPLE}.vcf
```

```bash
git add run_gatk.sh
git commit -m "Add GATK HaplotypeCaller script"
git push -u origin add-gatk-script
```
On GitHub, open a pull request from `add-gatk-script` into `main`. Click **Merge pull request**. Back in the terminal:
```bash
git switch main
git pull
git log --oneline
```
The merge commit now appears in local history.
