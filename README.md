# Whole-Exome-Analysis-for-Lung-Cancer-Biomarker-Discovery
## Project Overview

Lung cancer remains one of the leading causes of cancer-related mortality worldwide, largely due to late-stage diagnosis. Early detection through reliable molecular biomarkers has the potential to significantly improve patient outcomes.

This project performs a whole-exome sequencing (WES) analysis to identify somatic mutations and candidate biomarkers associated with early-stage lung cancer. By comparing tumor tissue against matched normal tissue, the study focuses on detecting tumor-specific genetic variations and analyzing their biological significance through pathway and functional analysis.

## Biological & Clinical Problem

- Most lung cancer cases are detected at advanced stages with limited treatment options.

- Early-stage biomarkers can enable:

    1. Earlier diagnosis

    2. Improved prognosis

    3. Targeted therapeutic strategies

- The objective of this study is to identify genetic variants and pathways that may serve as early-stage lung cancer biomarkers, supporting improved detection and intervention strategies.

## Project Objectives

- Identify somatic single nucleotide polymorphisms (SNPs) and insertions/deletions (Indels) associated with early-stage lung cancer.

- Compare tumor and normal paired samples to isolate tumor-specific mutations.

- Identify genes enriched with nonsynonymous missense mutations.

- Analyze biological pathways impacted by these mutations to assess clinical relevance.

## Dataset Description

The analysis uses paired-end whole-exome sequencing data:

### Normal Sample

- N_231335_R1_chr5.fastq.gz

- N_231335_R2_chr5.fastq.gz

### Tumor Sample

- T_231336_R1_chr5.fastq.gz

- T_231336_R2_chr5.fastq.gz

### Reference Genome

- hg19.chr5_12_17.fa (Chromosomes 5, 12, and 17)

## Analytical Workflow (End-to-End Pipeline)

### 1️⃣ Read Alignment

- Aligned paired-end reads from tumor and normal samples to the reference genome using BWA-MEM

- Generated SAM files capturing read alignments.

**Tool: BWA-MEM (v0.7.17)**

### 2️⃣ SAM to BAM Conversion & Sorting

- Converted SAM files to BAM format

- Sorted BAM files for efficient downstream processing

**Tool: SAMtools (v1.10.2)**

### 3️⃣ Duplicate Marking

- Identified and marked duplicate reads to reduce PCR and sequencing bias

- Generated deduplication metrics for quality assessment

**Tool: GATK MarkDuplicates (v4.1.0.0)**

### 4️⃣ Base Quality Score Recalibration (BQSR)

- Corrected systematic sequencing errors using known variant sites from dbSNP

- Applied recalibration to both tumor and normal samples

**Tools:**

- GATK BaseRecalibrator

- GATK ApplyBQSR

### 5️⃣ Somatic Variant Calling

- Compared recalibrated tumor and normal BAM files to identify tumor-specific variants

**Tool: GATK Mutect2**

### 6️⃣ Variant Filtering & Recovery

- Applied Mutect2 filtering

- Due to limited variant yield, variants were also generated separately for tumor and normal samples using BCFtools

- Ensured sufficient variant representation for downstream analysis

### 7️⃣ Variant Annotation

- Annotated variants using VEP (Variant Effect Predictor)

- Assessed functional impact on genes, proteins, and regulatory regions

### 8️⃣ Post-Processing & Filtering (Python)

- Variants were filtered to:

- Retain nonsynonymous missense mutations

- Keep tumor-specific variants only

- Identify the top 20 genes with the highest mutation counts

- Cross-reference genes known to be associated with lung cancer

## Key Findings

### Mutation Distribution

- Identified nonsynonymous missense variants across chromosomes 5, 12, and 17

- Chromosome 17 showed the highest mutation burden

### Top Mutated Genes

Among the top 20 mutated genes, several are strongly linked to lung cancer, including:

- KRAS

- CNTN1

- SCNN1A

- KLRK1

- USP5

- CHD4 (broad oncogenic relevance)

