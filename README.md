# Chrom_pro

- [Overview](#overview)
- [System Requirements](#system-requirements)
- [Installation Guide](#installation-guide)
- [Examples](#examples)
- [License](#license)

  

# Overview
Chromosome-scale genomes is fundamental to current genomic and post-genomic research, however, the assembly process remains complicated and challenging due to absence of a standardized automatic workflow. In this work, we have developed a multifunctional toolkit called ```Chrom-pro``` for de-novo chromosome-level genome assembly. This toolkit integrates commonly-used algorithms into a user-friendly, automatic workflow and is capable of assembling high-quality chromosome-level genomes using third-generation, second-generation, and Hi-C sequencing reads.  Besides, Chrom-pro was also equipped with practical functionality including genome quality assessment, comparative genomic analysis, and structural variation detection. ```Chrom_pro``` can be installed on Linux from Docker Hub.

![](.\Pipeline.jpg)





# System Requirements

## Hardware requirements
All tests were performed on an Ubuntu Linux 18.04.3 server equipped with two Intel Xeon processors (16 cores each, 48 threads total), 512 GB of RAM, and an additional 10TB of hard drive storage. For species with large genome sizes or those with a large volume of sequencing data, more memory and storage space may be required.

## Software requirements
Before using Chrom_pro, it is necessary to install Docker on the Linux system. The installation method for Docker can be found at this link: https://docs.docker.com/engine/install/.



# Installation Guide

### Install from Docker

```
sudo su

docker pull songweidocker/genomic_analysis:v1
```



# Examples
- De-novo chromosome-level genome assembly

  ```
  docker run -v /var/run/docker.sock:/var/run/docker.sock  -v $(pwd):/data  -w /data  songweidocker/genomic_analysis:v1  chrom_pro --help
  
  
  nohup docker run -v /var/run/docker.sock:/var/run/docker.sock  -v $(pwd):/data  -w /data  songweidocker/genomic_analysis:v1  chrom_pro    -TGS TGS_test.fastq  -NGS_1 NGS_test_1.fastq -NGS_2 NGS_test_2.fastq  -hic_1 HiC_test_1.fastq -hic_2 HiC_test_2.fastq -hic_Enzyme MBOI -chr_num 2 -assemble_method wtdbg2 -threads 60  -trim_organelle true -TGS_sequencing_method rs -GenomeSize 10m                      -HiC_sequence_align_method HiC-Pro  -Chromosome_assemble_method ALLHiC  &
  
  ```



- The quality assessment of chromosome assembly: 

  ```
  docker run -v /var/run/docker.sock:/var/run/docker.sock  -v $(pwd):/data  -w /data  songweidocker/genomic_analysis:v1  python /usr/bin/busco_calculate_plot.py assembled_genome.fasta
  ```

  

- Chromosome visualization:

  ```
  docker run -v /var/run/docker.sock:/var/run/docker.sock  -v $(pwd):/data  -w /data  songweidocker/genomic_analysis:v1 python /usr/bin/circos_plot.py -TGS TGS_test.fastq  -NGS_1 NGS_test_1.fastq -NGS_2 NGS_test_2.fastq  -hic_1 HiC_test_1.fastq -hic_2 HiC_test_2.fastq -hic_Enzyme MBOI -threads 8 -genome test_genome.fasta
  ```

  




- Collinearity analysis with reference genome:

  Before doing collinearity analysis, please ensure that both sets of chromosomes maintain a consistent orientation. Inverted complementary chromosomes may affect the accuracy of the final results.

  ```
  docker run -v /var/run/docker.sock:/var/run/docker.sock  -v $(pwd):/data  -w /data  songweidocker/genomic_analysis:v1 python /usr/bin/genome_comepare.py assembled_genome.fasta  reference_genome.fasta
  ```

  

- Genome structural variation detection:

  Before doing structural variation detection, please ensure that both sets of chromosomes maintain a consistent orientation. Inverted complementary chromosomes may affect the accuracy of the final results.

  ```
  docker run -v /var/run/docker.sock:/var/run/docker.sock  -v $(pwd):/data  -w /data  songweidocker/genomic_analysis:v1  /usr/bin/syri_plotsr.py assembled_genome.fasta reference_genome.fasta
  ```

  

# License

This project is covered under the **MIT License**.



# Reference

Song W, Ye T, Liu S, Shen D, Du Y, Yang Y, Lu Y, Jin H, Huo Y, Piao W et al: Chrom-pro: A User-Friendly Toolkit for De-novo Chromosome Assembly and Genomic Analysis. (2024) **bioRxiv**. [Link](https://www.biorxiv.org/content/10.1101/2024.03.02.583079v1)
