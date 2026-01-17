# Lesson 2 - Text Processing, Pipes, and Redirection

## Learning Objectives
- Explain how pipes connect commands in bioinformatics workflows.
- Use redirection to save command output into files.
- Count FASTQ reads with `wc` and the 4-line structure.
- Use `grep` to filter filenames and text.
- Use `sort` and `uniq` to summarize lists.

## Conceptual Overview
Pipes (`|`) send the output of one command directly into another. This is the foundation of many bioinformatics one-liners. Redirection (`>` and `>>`) lets you save command output to a file or append to it. Together, these tools let you build small, reproducible data summaries without opening a spreadsheet.

FASTQ files store reads in 4-line blocks:
1) Header
2) Sequence
3) `+` separator
4) Quality string

If you count lines with `wc -l`, you can estimate reads by dividing by 4.

## Worked Examples

### 1) Count reads with `wc`
`wc -l` uses the `-l` option to count lines in the file. Divide by 4 to estimate reads.
```bash
wc -l Training/short_reads/paired/SRR1553607_1.fastq
```
Output:
```
813780 Training/short_reads/paired/SRR1553607_1.fastq
```

### 2) Count Case samples using a pipe
`grep` filters lines that match the word `Case`, and `wc -l` counts those lines.
```bash
ls Training/short_reads/unpaired | grep Case | wc -l
```
Output:
```
3
```

### 3) Count Healthy samples using a pipe
`grep` filters lines that match the word `Healthy`, and `wc -l` counts those lines.
```bash
ls Training/short_reads/unpaired | grep Healthy | wc -l
```
Output:
```
4
```

### 4) Sort and deduplicate a list
`sort` arranges filenames alphabetically, and `uniq` removes duplicate adjacent lines.
```bash
ls Training/short_reads/unpaired | sort | uniq
```
Output:
```
SRR11282407_Case.fastq.gz
SRR11282408_Healthy.fastq.gz
SRR11282409_Healthy.fastq.gz
SRR11282410_Case.fastq.gz
SRR11282411_Healthy.fastq.gz
SRR11282412_Case.fastq.gz
SRR11282413_Healthy.fastq.gz
download.sh
```

### 5) Save output with redirection
`>` redirects output to a file and overwrites it if it exists.
No output appears because it is written to `Module_1_Linux/healthy_files.txt`.
```bash
ls Training/short_reads/unpaired | grep Healthy > Module_1_Linux/healthy_files.txt
```

`>>` appends output to the end of a file.
After this, the file contains Healthy and Case filenames, in the order you wrote them.
```bash
ls Training/short_reads/unpaired | grep Case >> Module_1_Linux/healthy_files.txt
```

## Exercises
1) Use `wc -l` to count lines in `Training/short_reads/paired/SRR1553607_2.fastq`. Estimate the read count by dividing by 4.

2) Use a pipe to count how many filenames in `Training/short_reads/unpaired` contain `Healthy`. Expected output: `4`.

3) Use a pipe to count how many filenames in `Training/short_reads/unpaired` contain `Case`. Expected output: `3`.

4) Use `sort` and `uniq` to list unique filenames in `Training/short_reads/unpaired`.

5) Challenge: Use `wc -l` with both paired files in one command and redirect the output to `Module_1_Linux/read_line_counts.txt`.

## Solutions

### Solution 1
`wc -l` counts lines in the file. Divide by 4 to estimate reads.
```bash
wc -l Training/short_reads/paired/SRR1553607_2.fastq
```
Output:
```
813780 Training/short_reads/paired/SRR1553607_2.fastq
```

### Solution 2
`grep` filters for Healthy filenames, and `wc -l` counts them.
```bash
ls Training/short_reads/unpaired | grep Healthy | wc -l
```
Output:
```
4
```

### Solution 3
`grep` filters for Case filenames, and `wc -l` counts them.
```bash
ls Training/short_reads/unpaired | grep Case | wc -l
```
Output:
```
3
```

### Solution 4
`sort` orders the list, and `uniq` removes duplicates.
```bash
ls Training/short_reads/unpaired | sort | uniq
```
Output:
```
SRR11282407_Case.fastq.gz
SRR11282408_Healthy.fastq.gz
SRR11282409_Healthy.fastq.gz
SRR11282410_Case.fastq.gz
SRR11282411_Healthy.fastq.gz
SRR11282412_Case.fastq.gz
SRR11282413_Healthy.fastq.gz
download.sh
```

### Solution 5
`wc -l` can take multiple files and prints one line per file plus a total. No output appears because it is written to `Module_1_Linux/read_line_counts.txt`.
```bash
wc -l Training/short_reads/paired/SRR1553607_1.fastq Training/short_reads/paired/SRR1553607_2.fastq > Module_1_Linux/read_line_counts.txt
```
