# Module 3 - Omics Analysis
## Module Overview
This module marks the transition from computational literacy to biological discovery. Having mastered the command line (Module 1) and environment management (Module 2), we now apply these skills to process high-throughput sequencing data. We will execute an end-to-end pipeline to transform raw, noisy signals into structured biological insights. 

## Learning Objectives

* **Systemic Understanding:** Comprehend the Omics hierarchy and the flow of information from genotype to phenotype.
* **Data Integrity:** Master the "Big Four" bioinformatic file formats (FASTQ, FASTA, BAM, VCF) and their technical specifications.
* **Standard Operating Procedures (SOPs):** Execute reproducible workflows for Quality Control, Trimming, Alignment, and Variant Discovery.

## Lessons 
Each lesson below is a standalone guide containing technical theory and executable code blocks.

Lesson 1: The Omics Hierarchy & File Architecture.
  *    Focus: Information theory and technical data containers.

Lesson 2: Precision Quality Control (QC)
*    Focus: Statistical assessment of raw data "health" using FastQC and MultiQC.

Lesson 3: Digital Surgery (Read Trimming)
*   Focus: Pre-processing raw reads with Trimmomatic to remove adapter contamination.
  
Lesson 4: Genomic Mapping & Alignment
*   Focus: Reconstructing the genome by aligning short reads to a reference using BWA-MEM and Samtools.

Lesson 5: Variant Discovery & Interpretation
* Focus: Identifying Single Nucleotide Polymorphisms (SNPs) and Indels using BCFtools.

## Module Toolbox
To follow these lessons, ensure your module_3_omics environment is active.

```
Bash
# Create and activate the specialized Omics environment
conda create -n module_3_omics fastqc multiqc trimmomatic bwa samtools bcftools -y
conda activate module_3_omics
```