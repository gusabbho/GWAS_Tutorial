# PLINK Data formats

## Text Format PLINK Files

1. **The .ped File:** 
The .ped file stores both individual information and genotype data in a single file. This file is tab-delimited and has the following structure:

***Family ID***: Unique identifier for each family.

***Individual ID:*** Unique identifier for each individual within the family.

***Paternal ID:*** ID of the father (if known).

***Maternal ID:*** ID of the mother (if known).

***Sex:*** Sex of the individual (1 for male, 2 for female).

***Phenotype:*** Observed trait or condition, such as disease status (commonly coded as -9, 1, or 2).

***Genotype Data:*** Following these columns, each genotype at each SNP is represented as a pair of alleles (e.g., A T, G G), repeated for all SNPs.

2. **The .map File:** 
The .map file is simpler and provides information about the markers (SNPs). Each row in a .map file corresponds to a SNP, with columns:

***Chromosome:*** The chromosome number for each SNP.

***SNP ID:*** Unique identifier for each SNP.

***Genetic Distance:*** Distance in centimorgans (optional, often set to 0).

***Base-pair Position:*** Physical position on the chromosome in base pairs.


## Binary File Trio



In PLINK, the term "binary file trio" refers to a set of three files that, together, store genetic data in a compact, efficient format for analysis. These files are:

1. ***.bed file:*** The binary genotype file. This is a true binary file, meaning it contains genotype data in a compressed, machine-readable format. It encodes information about the genotypes (like AA, AB, BB) for each sample at each SNP (single nucleotide polymorphism) in a highly compressed way, which saves disk space and speeds up analysis.

2. ***.bim file:*** The extended MAP file. This is a plain text file with information about each SNP or genetic marker, such as its chromosome number, ID, genetic position, physical position, and alleles. This file complements the .bed file by providing the necessary details about each SNP.
 
3. ***.fam file:*** The family information file. This is also a plain text file containing metadata about each individual in the study, such as family ID, individual ID, parental IDs, sex, and phenotype information (such as disease status). The .fam file aligns with the .bed file to ensure that each row of genotype data corresponds to the correct individual and their family information.


### **The Purpose of Binary File Trio**

Together, these three files make up the binary PLINK dataset:

* The .bed file holds the main genotype data in binary form, making it faster to load and analyze.

* The .bim and .fam files provide all the necessary annotations for the data in the .bed file, describing the genetic markers and sample details.

### Why Use a "Binary File Trio"?
PLINK’s binary file format is designed for performance and efficiency. Instead of storing the entire dataset in a single large file, the data is split into separate files:

* The .bed file saves space by storing only genotypes in binary form.

* The .bim and .fam files provide easy-to-access metadata in a readable format for humans and software.


This modular structure allows PLINK to load and process data much faster than if everything were stored in a single, large, text-based file.






### Comparison of Text (.ped/.map) vs. Binary (.bed/.bim/.fam) Files

| Feature                  | .ped/.map Files                              | .bed/.bim/.fam Files                             |
|--------------------------|----------------------------------------------|--------------------------------------------------|
| **Format Type**          | Text                                        | Binary                                           |
| **Genotype Data**        | Stored in **.ped** file                     | Stored in **.bed** file                          |
| **Marker Information**   | Stored in **.map** file                     | Stored in **.bim** file                          |
| **Sample Information**   | Family and phenotype in **.ped** file       | Family and phenotype in **.fam** file            |
| **File Size Efficiency** | Larger (less efficient)                     | Smaller (more efficient for large datasets)      |
| **Speed of Processing**  | Slower (less optimized)                     | Faster (optimized for analysis in PLINK)         |


In short, .ped/.map files are a more human-readable, flexible format, while .bed/.bim/.fam files are more optimized for large datasets, as they store genotype data in binary form to save space and speed up analyses.

## Following picture illustrates the difference between data formats.

![Figure 1. Overview of various commonly used PLINK files. Retrieved from ResearchGate by Alkes Price, 2024. Copyright 2024 Alkes Price.](https://www.researchgate.net/publication/323424714/figure/fig3/AS:667766705098757@1536219397189/Overview-of-various-commonly-used-PLINK-files-SNP-single-nucleotide-polymorphism.png)
<small>*Figure 1. Overview of various commonly used PLINK files. Retrieved from ResearchGate by Alkes Price, 2024. Copyright 2024 Alkes Price.*</small>



# PLINK 2.0

 PLINK 2.0 introduces a new binary file format, designed to provide more detailed information on genetic variants and improve data handling and quality control in genetic studies. Here’s an overview of the PLINK 2.0 file trio:

**.pvar File:**

Analogous to the .bim file in PLINK 1.0.
Stores genetic marker information, such as SNP ID, chromosome number, and base-pair position.
Contains additional columns for imputation quality scores (e.g., QUAL and INFO), which help assess the accuracy of imputed genetic variants.

**.psam File:**

Similar to the .fam file in PLINK 1.0.
Contains individual metadata, including unique identifiers, phenotypes, and more.
Includes extra fields related to genetic variant quality, providing a more comprehensive sample profile.

**.pgen File:**

Replaces the .bed file and is a compressed, binary file that stores genotype probabilities, rather than simple genotype values.
Contains likelihood estimates of each genotype for imputed variants, enhancing data quality control and analysis precision in GWAS (Genome-Wide Association Studies).

| **PLINK 2.0 File** | **Description**                                                                                          | **Similar to (PLINK 1.0)** |
|--------------------|----------------------------------------------------------------------------------------------------------|----------------------------|
| **.pvar**          | Stores genetic marker data, including SNP info and imputation quality metrics like `QUAL` and `INFO`.    | .bim                       |
| **.psam**          | Contains sample information, with extended fields for imputation quality and other individual attributes. | .fam                       |
| **.pgen**          | Compressed binary file storing genotype probabilities for imputed variants. Not readable in text editors. | .bed                       |


**Key Benefits of PLINK 2.0**

* Enhanced Quality Control: Imputation quality metrics aid in identifying data issues.

* Genotype Probabilities: pgen files allow probabilistic genotype assignments, improving analyses in genetic research.
 
* Data Efficiency: Compressed file formats optimize storage and processing for large-scale datasets in genetic studies.

# VCF files (variant call format)

The VCF (Variant Call Format) is a versatile and widely used file format in bioinformatics for storing genomic information, including genotyped, imputed, and sequencing data. It was designed for handling extensive data from large projects, like the 1000 Genomes Project. VCF files are text-based and can be read with a text editor (though it's not recommended for large files) or analyzed using software.

Structure of a VCF File
* Preamble: Contains metadata lines (prefixed with ##) detailing information such as the file version, source, and reference genome.

* Header: A single line (prefixed with #) listing columns for data that follows.

* Data Rows: Each row provides genetic data for a particular genomic position, including the reference allele, alternative alleles, genotype quality, and other relevant metrics. This format can accommodate a wide range of variant types, such as SNPs, indels, and CNVs.


VCF files are flexible and can store various key-value fields, like chromosome position, quality scores, and sample-specific genotypes. They are compatible with software like PLINK for conversion and analysis, making them ideal for use i
n GWAS and other genomic studies.

| **VCF Component**      | **Description**                                                                                              |
|------------------------|--------------------------------------------------------------------------------------------------------------|
| **Preamble (##)**      | Metadata about file format, source, reference genome, and filters.                                           |
| **Header (#)**         | Lists column headers for the data rows (e.g., chromosome, position, reference allele).                       |
| **Data Rows**          | Genomic data for each position, including columns for `CHROM`, `POS`, `ID`, `REF`, `ALT`, `QUAL`, `FILTER`. |
| **Genotype Information** | Contains genotype-specific fields (e.g., `GT` for genotype, `GQ` for genotype quality) for each sample.   |
| **INFO**               | Key-value pairs providing additional variant details, such as allele frequency and depth of coverage.        |
| **Compatibility**      | Compatible with various software for analysis and conversion, such as PLINK.                                 |

**Key Fields in VCF Data Rows**

| **Column**   | **Description**                                                    |
|--------------|--------------------------------------------------------------------|
| **Family ID**| Identifier for the family.                                         |
| **Individual ID** | Identifier for the individual.                                  |
| **Paternal ID**   | Identifier for the father of the individual.                    |
| **Maternal ID**   | Identifier for the mother of the individual.                    |
| **Sex**          | Sex of the individual (1=male, 2=female).                      |
| **Phenotype**    | Phenotype of the individual (1=control, 2=case, -9=missing).  |
| **Genotype Data**| Genotype data for the individual (two columns per SNP).        |


This table captures the main components of VCF files and highlights their versatility and compatibility with various bioinformatics tools.