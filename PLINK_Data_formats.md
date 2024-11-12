# PLINK Data formats
In PLINK, the term "binary file trio" refers to a set of three files that, together, store genetic data in a compact, efficient format for analysis. These files are:

1. .bed file: The binary genotype file. This is a true binary file, meaning it contains genotype data in a compressed, machine-readable format. It encodes information about the genotypes (like AA, AB, BB) for each sample at each SNP (single nucleotide polymorphism) in a highly compressed way, which saves disk space and speeds up analysis.

2. .bim file: The extended MAP file. This is a plain text file with information about each SNP or genetic marker, such as its chromosome number, ID, genetic position, physical position, and alleles. This file complements the .bed file by providing the necessary details about each SNP.
 
3. .fam file: The family information file. This is also a plain text file containing metadata about each individual in the study, such as family ID, individual ID, parental IDs, sex, and phenotype information (such as disease status). The .fam file aligns with the .bed file to ensure that each row of genotype data corresponds to the correct individual and their family information.


### Purpose of the Binary File Trio
Together, these three files make up the binary PLINK dataset:

* The .bed file holds the main genotype data in binary form, making it faster to load and analyze.

* The .bim and .fam files provide all the necessary annotations for the data in the .bed file, describing the genetic markers and sample details.

### Why Use a "Binary File Trio"?
PLINK’s binary file format is designed for performance and efficiency. Instead of storing the entire dataset in a single large file, the data is split into separate files:

* The .bed file saves space by storing only genotypes in binary form.

* The .bim and .fam files provide easy-to-access metadata in a readable format for humans and software.


This modular structure allows PLINK to load and process data much faster than if everything were stored in a single, large, text-based file.

![Figure 1. Overview of various commonly used PLINK files. Retrieved from ResearchGate by Alkes Price, 2024. Copyright 2024 Alkes Price.](https://www.researchgate.net/publication/323424714/figure/fig3/AS:667766705098757@1536219397189/Overview-of-various-commonly-used-PLINK-files-SNP-single-nucleotide-polymorphism.png)
<small>*Figure 1. Overview of various commonly used PLINK files. Retrieved from ResearchGate by Alkes Price, 2024. Copyright 2024 Alkes Price.*</small>

sg






