# Lesson 1: The Omics Hierarchy & File Architecture

1. Biological Context: The Omics Layers
Omics refers to the collective technologies used to explore the roles, relationships, and actions of various types of molecules in an organism. We view this through the lens of the Central Dogma of Molecular Biology:

* Genomics (DNA-Seq): The study of the complete set of DNA (the genome). It focuses on the static "blueprint" identifying mutations like SNPs (Single Nucleotide Polymorphisms) that might predispose an organism to a trait or disease.

* Transcriptomics (RNA-Seq): The study of the transcriptome—the complete set of RNA transcripts produced by the genome. This layer tells us which genes are "turned on" or "expressed" under specific conditions.

2. Technical Definitions: The "Big Four" File Formats

I. FASTQ: The Raw Sequence 
The FASTQ file is the raw output from a sequencing machine (like Illumina or Oxford Nanopore).

Definition: A text-based format that stores both a biological sequence and its corresponding quality scores.

Structure (The 4-Line Rule):

Line 1: Header (starts with @) containing sequence ID and hardware info.

Line 2: The actual sequence (A, C, T, G, N).

Line 3: Separator (starts with +).

Line 4: Phred Quality Scores (encoded in ASCII characters).

II. FASTA: The Reference Genome (The Map)

Definition: A simple text format for representing either nucleotide or peptide sequences.

Usage: We use these as "Reference Genomes." When we analyze your Training data, we compare your raw reads (FASTQ) against this gold-standard map (FASTA).

Structure: Starts with a > followed by the sequence name/description.

III. SAM/BAM: The Alignment Address (The Connection)

SAM (Sequence Alignment Map): A human-readable text file that shows where each FASTQ read "lives" on the FASTA reference.

BAM (Binary Alignment Map): The exact same data as a SAM file, but compressed into a binary format that computers can read incredibly fast.

Essential Rule: Always use BAM for storage to save disk space, and use Samtools to "view" it.

IV. VCF: The Variant Call Format (The Truth)

Definition: A tab-delimited text file used to store gene sequence variations.

Content: It identifies the chromosome, the exact position, and the change (e.g., the reference has an A, but your sample has a G).

3. Hands-on Conceptual Exercise
Open your terminal, navigate to your Training directory, and let's "look" at the data architecture.

Check your Raw Data:
```
Bash
cd ~/Training/short_reads/unpaired
# Peek at the first 4 lines of a FASTQ file
zcat SRR11282408_Healthy.fastq.gz | head -n 4
```
The Observation:
Look at Line 4. Those symbols (!, @, #, B) are actually numbers representing the computer's confidence in that base call.

A # usually means the computer is very unsure, while a I or J means high confidence.