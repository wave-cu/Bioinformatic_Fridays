# Lesson 4 - SSH and Remote Work

## Learning Objectives
- Explain what SSH is and why bioinformaticians use it.
- Distinguish between local and remote filesystems.
- Write correct `ssh` and `scp` command syntax.
- Diagnose common SSH and SCP mistakes.

## Conceptual Overview
SSH (Secure Shell) lets you log into a remote server and run commands as if you were sitting at that machine. Bioinformatics often uses remote servers or clusters because they have more CPU, memory, and storage than a laptop.

When you connect with SSH, you are working in the remote filesystem. Your local files are still on your computer. Use `scp` to copy files between local and remote systems. Keep your project organized so you always know where your data and results live.

Common best practices:
- Avoid heavy analyses on your laptop; use remote servers for big jobs.
- Keep clear folder names and document your working directory.
- Use keys or passwords safely; never share private keys.

## Worked Examples

### 1) Basic SSH login (conceptual)
`ssh user@host` starts a secure login session to a remote server.
If the connection succeeds, you might see a login banner and then a remote prompt. You are now working on the remote system.
```bash
ssh username@bioinfo.example.edu
```
```
Welcome to Bioinfo Server
username@bioinfo:~$
```

### 2) Run a command on the remote server
Quoting a command after `ssh` runs it remotely and prints the remote output locally.
```bash
ssh username@bioinfo.example.edu "pwd"
```
Example output:
```
/home/username
```

### 3) Copy a file to the remote server with `scp`
`scp local_file user@host:remote_path` copies a file from local to remote.
```bash
scp Module_1_Linux/notes.txt username@bioinfo.example.edu:~/bioinformatics/
```

### 4) Copy results back to your laptop
`scp user@host:remote_file local_path` copies a file from remote to local.
```bash
scp username@bioinfo.example.edu:~/bioinformatics/results.txt Module_1_Linux/
```

## Exercises
1) Write the SSH command to log in as user `student42` to a server named `training.cluster.edu`.

2) Write the command to copy `Module_1_Linux/read_line_counts.txt` to `~/week1/` on the remote server.

3) Write the command to copy `~/week1/summary.txt` from the remote server back into your local `Module_1_Linux/`.

4) Identify the mistake in each broken command and fix it:
- `ssh student42@training.cluster.edu:~/week1/`
- `scp Module_1_Linux/read_line_counts.txt student42@training.cluster.edu`
- `scp student42@training.cluster.edu ~/week1/summary.txt Module_1_Linux/`

5) Challenge: Explain the difference between running `ls` after you SSH and running `ls` locally before you SSH.

## Solutions

### Solution 1
Use `user@host` format.
```bash
ssh student42@training.cluster.edu
```

### Solution 2
Copy a local file to a remote folder using `user@host:remote_path`.
```bash
scp Module_1_Linux/read_line_counts.txt student42@training.cluster.edu:~/week1/
```

### Solution 3
Copy a remote file to a local folder using `user@host:remote_file`.
```bash
scp student42@training.cluster.edu:~/week1/summary.txt Module_1_Linux/
```

### Solution 4
Fix the syntax errors in each command. Mistakes explained: `ssh` does not take a remote path; `scp` needs a destination path; the third command is missing the colon after the hostname in the remote path.
```bash
ssh student42@training.cluster.edu
scp Module_1_Linux/read_line_counts.txt student42@training.cluster.edu:~/
scp student42@training.cluster.edu:~/week1/summary.txt Module_1_Linux/
```

### Solution 5
After SSH, commands run on the remote server and show the remote filesystem. Local commands run on your own machine and show local files.
