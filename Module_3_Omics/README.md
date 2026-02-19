# Module 3 - Omics Analysis
## Module Overview
This module marks the transition from computational literacy to biological discovery. Having mastered the command line (Module 1) and environment management (Module 2), we now apply these skills to process high-throughput sequencing data. We will execute an end-to-end pipeline to transform raw, noisy signals into structured biological insights. 

## Learning Objectives

* **Systemic Understanding:** Comprehend the Omics hierarchy and the flow of information from genotype to phenotype.
* **Data Integrity:** Master the "Big Four" bioinformatic file formats (FASTQ, FASTA, BAM, VCF) and their technical specifications.
* **Standard Operating Procedures (SOPs):** Execute reproducible workflows for long-read de novo assembly, reference-based mapping, and consensus generation.

## Lessons 
Each lesson below is a standalone guide containing technical theory and executable code blocks.

Lesson 1: The Omics Hierarchy & File Architecture.
  *    Focus: Information theory and technical data containers.

Lesson 2: Precision Quality Control (QC)
*    Focus: Statistical assessment of raw data "health" using FastQC and MultiQC.

Lesson 3: Genome Assembly Foundations
*   Focus: What genome assembly is, why it matters, de novo vs reference-based strategies, software for short and long reads, and an end-to-end assembly pipeline overview.
  
Lesson 4: De novo Assembly (Hands-on)
*   Focus: Long-read de novo assembly using `barcode57` and `barcode58`, including host-read filtering, Flye assembly, and BLAST annotation workflow.

Lesson 5: Reference-Based Assembly
* Focus: Long-read reference-guided assembly against combined ACMV DNA-A and DNA-B, with mapping QC and consensus sequence generation.

## Module Toolbox
To follow Lessons 1-2, ensure your base omics environment is active. For Lessons 4-5, use the dedicated `bioinfo' environment.