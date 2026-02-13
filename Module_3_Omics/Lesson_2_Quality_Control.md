# Lesson 2: Precision Quality Control (QC)

1. ****The Biological Narrative:** "Cleaning the Lens"**Before a satellite can take a clear picture of the Earth, its lens must be free of dust. In bioinformatics, "dust" comes in the form of sequencing artifacts:
 
* **Adapter Contamination:** Small pieces of synthetic DNA used to attach your sample to the sequencer that are accidentally read as part of the sequence.
 
* **Base Calling Errors:** The sequencer becomes "tired" or less accurate toward the end of a long run, leading to incorrect base calls.


2. **Technical Definition:** **The Phred Quality Score (Q)**:
To measure "cleanliness," we use the Phred Score. This is a logarithmic scale that represents the probability ($P$) that a specific base call is incorrect.


The formula for the Phred score is:
        
    Q = -10log{10}(P)
```
Phred Score (Q)	Probability of Incorrect Base (P)	Base Call Accuracy
10	                1 in 10	                              90%
20	                1 in 100	                      99%
30	                1 in 1,000	                      99.9% (Gold Standard)
```
3. **The Professional Toolkit: FastQC & MultiQC**

We use two industry-standard tools to assess our ``Training`` data.
* FastQC: Generates a detailed report for a single sample.
* MultiQC: Aggregates many FastQC reports into a single, interactive dashboard essential for comparing your groups.

4. Hands-on Execution: Analyzing your Training Data
Follow these steps to run your first QC pipeline on the samples in your ``Training`` directory.

**Step 1: Activate the Environment**
```
Bash
conda activate bioinfo
```
**Step 2: Run FastQC**

We will run FastQC on all the compressed files in your ```unpaired``` directory at once.
```
Bash
# Create a folder for the results
mkdir -p ~/Training/QC_Results

fastqc --help

# Run FastQC on all .fastq.gz files
fastqc ~/Training/short_reads/unpaired/*.fastq.gz -o ~/Training/QC_Results
```
**Step 3: Aggregate with MultiQC**

Instead of opening 7 different files, we create one master report.
```
Bash
cd ~/Training/QC_Results

mkdir multiqc_results
multiqc --help

multiqc ./multiqc_results 
```
5. **Interpreting the Results**

When you open the multiqc_report.html in your browser, focus on these three sections:
1. Per Base Sequence Quality: If the "tails" of your reads (the right side of the graph) dip into the Red Zone (below Q20), we must trim them in Lesson 3. 
2. Adapter Content: If this graph shows a rising line, it means your "Digital Surgery" (Trimming) will be mandatory to remove those synthetic sequences.
3. Overrepresented Sequences: This often identifies contaminants or highly abundant transcripts in RNA-Seq data.









