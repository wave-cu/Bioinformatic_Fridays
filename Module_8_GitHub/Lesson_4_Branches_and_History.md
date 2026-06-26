# Lesson 4 — Branches and History

## Learning Objectives
- Explain what a branch is and why branches are useful.
- Create and switch between branches with `git branch` and `git switch`.
- Merge a branch back into `main` with `git merge`.
- Resolve a simple merge conflict.
- Inspect history with `git log`, `git show`, and `git diff`.
- Undo mistakes with `git restore` and understand when to use it.

## Conceptual Overview

### What is a branch?
A branch is an independent line of development. Think of the `main` branch as the stable, working version of your project. When you want to add a new feature or try something experimental, you create a branch. Changes on that branch do not affect `main` until you deliberately merge them.

This is extremely useful in bioinformatics:
- Try a different trimming parameter without breaking your current pipeline.
- Add a new analysis step while keeping the original script intact.
- Collaborate: two people work on different branches and merge when ready.

### How branches work internally
A branch is simply a pointer to a commit. When you commit on a branch, the pointer moves forward. When you merge, Git combines the two lines of commits.

```
main:      A ── B ── C ── F (merge)
                  \       /
new-feature:       D ── E
```

### HEAD
`HEAD` is a special pointer that shows which commit you are currently looking at. Normally it points to the tip of your current branch.

---

## Worked Examples

### 1) See existing branches

```bash
cd ~/projects/my_analysis
git branch
```

Output:
```
* main
```

The `*` marks the currently active branch. Right now there is only `main`.

---

### 2) Create a new branch

`git branch` with a name creates a new branch without switching to it.

```bash
git branch add-trimming-script
git branch
```

Output:
```
  add-trimming-script
* main
```

The `*` is still on `main` — you have not switched yet.

---

### 3) Switch to the new branch

`git switch` changes your active branch.

```bash
git switch add-trimming-script
```

Output:
```
Switched to branch 'add-trimming-script'
```

Confirm:
```bash
git branch
```

Output:
```
* add-trimming-script
  main
```

---

### 4) Create a shortcut: create and switch in one command

You can combine the last two steps:
```bash
git switch -c some-other-branch
```

The `-c` flag means "create". For now, delete this extra branch and stay on `add-trimming-script`:
```bash
git switch add-trimming-script
git branch -d some-other-branch
```

Output:
```
Deleted branch some-other-branch (was f4a52b1).
```

---

### 5) Add a file on the new branch

Create a trimming script:

```bash
nano run_trimmomatic.sh
```

Type the following, then save and exit:
```bash
#!/usr/bin/env bash
# Trim adapters from paired-end reads using Trimmomatic

SAMPLE="SRR1553607"
THREADS=4

trimmomatic PE \
    Training/short_reads/paired/${SAMPLE}_1.fastq \
    Training/short_reads/paired/${SAMPLE}_2.fastq \
    trimmed/${SAMPLE}_1_paired.fastq \
    trimmed/${SAMPLE}_1_unpaired.fastq \
    trimmed/${SAMPLE}_2_paired.fastq \
    trimmed/${SAMPLE}_2_unpaired.fastq \
    ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 \
    LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36 \
    -threads ${THREADS}
```

Stage and commit:
```bash
git add run_trimmomatic.sh
git commit -m "Add Trimmomatic script for paired-end trimming"
```

Output:
```
[add-trimming-script a1b2c3d] Add Trimmomatic script for paired-end trimming
 1 file changed, 18 insertions(+)
 create mode 100755 run_trimmomatic.sh
```

---

### 6) Compare branches

Switch back to `main` and notice that `run_trimmomatic.sh` does not exist there yet:

```bash
git switch main
ls
```

Output:
```
README.md  environment.md  methods.md  notes.md  run_fastqc.sh  sample_list.txt
```

`run_trimmomatic.sh` only exists on `add-trimming-script`. Switch back:
```bash
git switch add-trimming-script
ls
```

Output:
```
README.md  environment.md  methods.md  notes.md  run_fastqc.sh  run_trimmomatic.sh  sample_list.txt
```

---

### 7) Merge the branch into main

When you are satisfied with the work on a branch, merge it into `main`. First switch to `main`, then merge:

```bash
git switch main
git merge add-trimming-script
```

Output:
```
Updating f4a52b1..a1b2c3d
Fast-forward
 run_trimmomatic.sh | 18 ++++++++++++++++++
 1 file changed, 18 insertions(+)
 create mode 100755 run_trimmomatic.sh
```

"Fast-forward" means `main` simply moved its pointer forward to the new commit — no conflict to resolve. Now `run_trimmomatic.sh` exists on `main`:

```bash
ls
```

Output:
```
README.md  environment.md  methods.md  notes.md  run_fastqc.sh  run_trimmomatic.sh  sample_list.txt
```

Push the merged `main` to GitHub:
```bash
git push
```

---

### 8) Delete the merged branch

After merging, the branch is no longer needed:

```bash
git branch -d add-trimming-script
```

Output:
```
Deleted branch add-trimming-script (was a1b2c3d).
```

---

### 9) Simulate and resolve a merge conflict

Conflicts happen when two branches edit the same part of the same file. Let us simulate one.

Create a branch and edit the first line of `notes.md`:
```bash
git switch -c edit-notes-branch
nano notes.md
```

Change the first line from `## Session notes` to `## Analysis notes`. Save and exit.
```bash
git add notes.md
git commit -m "Rename notes header to Analysis notes"
```

Now switch back to `main` and make a different edit to the same line:
```bash
git switch main
nano notes.md
```

Change the first line to `## Lab notes`. Save and exit.
```bash
git add notes.md
git commit -m "Rename notes header to Lab notes"
```

Try to merge:
```bash
git merge edit-notes-branch
```

Output:
```
Auto-merging notes.md
CONFLICT (content): Merge conflict in notes.md
Automatic merge failed; fix conflicts then commit the result.
```

Open the file to see the conflict markers:
```bash
cat notes.md
```

Output:
```
<<<<<<< HEAD
## Lab notes
=======
## Analysis notes
>>>>>>> edit-notes-branch

- Installed FastQC
- Downloaded SRR1553607 dataset
- FastQC completed successfully on all samples
- Pushed repository to GitHub successfully
```

The section between `<<<<<<< HEAD` and `=======` is what `main` has. The section between `=======` and `>>>>>>> edit-notes-branch` is what the other branch has. You must decide which version to keep.

Edit the file to remove the conflict markers and keep your chosen version:
```bash
nano notes.md
```

Delete the three conflict marker lines and keep only:
```
## Session notes

- Installed FastQC
...
```

Save and exit. Then stage and commit to complete the merge:
```bash
git add notes.md
git commit -m "Merge edit-notes-branch — keep original header"
```

Output:
```
[main e9f3a1c] Merge edit-notes-branch — keep original header
```

Delete the merged branch:
```bash
git branch -d edit-notes-branch
```

---

### 10) Inspect a specific commit

`git show` prints the full details and diff of a single commit. Use the hash from `git log --oneline`:

```bash
git log --oneline
```

Output (abbreviated):
```
e9f3a1c (HEAD -> main) Merge edit-notes-branch — keep original header
a1b2c3d Add Trimmomatic script for paired-end trimming
...
```

```bash
git show a1b2c3d
```

Output:
```
commit a1b2c3d...
Author: Your Name <you@example.com>
Date:   Thu Jun 26 10:00:00 2025 +0000

    Add Trimmomatic script for paired-end trimming

diff --git a/run_trimmomatic.sh b/run_trimmomatic.sh
new file mode 100755
...
```

---

### 11) Undo changes before staging

You edited `README.md` but realise the change was a mistake and you have not staged it yet:

```bash
nano README.md
```

Add an accidental line: `THIS IS A MISTAKE`. Save and exit.

Check the diff:
```bash
git diff README.md
```

Output shows the accidental line with a `+`. Discard the change:
```bash
git restore README.md
```

No output is expected. Verify:
```bash
git diff README.md
```

No output — the file is back to its last committed state.

---

### 12) Undo a staged change

Stage the accidental change first to simulate the scenario:
```bash
nano README.md
```
Add `THIS IS A MISTAKE` again. Save and exit.
```bash
git add README.md
git status
```

Output:
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md
```

Unstage it (moves the file back to the working directory without discarding changes):
```bash
git restore --staged README.md
git status
```

Output:
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md
```

Now discard the change entirely:
```bash
git restore README.md
```

---

## Exercises

1) Create a branch called `add-alignment-script`. On that branch, create a file called `run_bwa.sh` with a short BWA alignment command. Commit it and then merge it into `main`.

2) After merging, run `git log --oneline`. Notice the new commit appears. Push `main` to GitHub and check the GitHub commits page.

3) Run `git branch`. How many branches do you have now? Delete any merged branches.

4) Use `git show` on the commit that added `run_fastqc.sh`. What files changed?

5) Challenge: Make two branches that both edit different lines of `methods.md`. Merge them both into `main` without a conflict. Explain why there was no conflict.

---

## Solutions

### Solution 1
```bash
git switch -c add-alignment-script
nano run_bwa.sh
```
File content:
```bash
#!/usr/bin/env bash
# Align paired-end reads to a reference genome using BWA-MEM

bwa mem reference.fa \
    trimmed/SRR1553607_1_paired.fastq \
    trimmed/SRR1553607_2_paired.fastq \
    > aligned/SRR1553607.sam
```

```bash
git add run_bwa.sh
git commit -m "Add BWA-MEM alignment script"
git switch main
git merge add-alignment-script
```

### Solution 2
```bash
git push
```
On GitHub, the commits page shows the merge and the new commit from the branch.

### Solution 3
```bash
git branch
```
Output:
```
* main
```
After deleting merged branches only `main` remains.

### Solution 4
Find the hash of the commit that added `run_fastqc.sh`:
```bash
git log --oneline
```
Find the line `Add sample list and FastQC script`, note the hash (e.g. `c1e20f3`), then:
```bash
git show c1e20f3
```
Output shows that `run_fastqc.sh` and `sample_list.txt` were created.

### Solution 5
Two branches editing different lines of the same file produce no conflict because Git can combine non-overlapping edits automatically. A conflict only occurs when two branches change the same lines.
