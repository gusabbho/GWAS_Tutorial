
# PLINK Command Line Script for Genotype Data Processing


## Command Explanations

1. **Converting Binary PLINK Files to Human-Readable Format**  
   This command converts a binary PLINK file into a human-readable format.

   - **Command**:  
     ```bash
     plink --bfile hapmap-ceu --recode --out hapmap-ceu
     ```
   - **Arguments**:  
     - ***--bfile hapmap-ceu***: Specifies the prefix of the binary file to be used as input. Here, `hapmap-ceu` is the prefix for files like `hapmap-ceu.bed`, `hapmap-ceu.bim`, and `hapmap-ceu.fam`.
     - ***--recode***: Tells PLINK to convert the binary data into a more accessible format, typically outputting `.ped` and `.map` files.
     - ***--out hapmap-ceu***: Defines the output prefix, so output files will be named `hapmap-ceu.ped` and `hapmap-ceu.map`.

   This command is useful when you need human-readable data for downstream analysis or when sharing data with collaborators who might need non-binary formats.

2. **Converting VCF Files to PLINK Binary Format**  
   This command converts a VCF file (Variant Call Format) into PLINK's binary format.

   - **Command**:  
     ```bash
     plink --vcf ALL.chr21.vcf.gz --make-bed --out test_vcf
     ```
   - **Arguments**:  
     - ***--vcf ALL.chr21.vcf.gz***: Specifies the VCF file to use as input. Here, `ALL.chr21.vcf.gz` represents a compressed VCF file for chromosome 21.
     - ***--make-bed***: Instructs PLINK to create binary output files in `.bed`, `.bim`, and `.fam` formats.
     - ***--out test_vcf***: Specifies the prefix for the output files, so resulting files will be `test_vcf.bed`, `test_vcf.bim`, and `test_vcf.fam`.

   This command is useful for converting VCF data into a more manageable format for large-scale genotype analyses.

3. **Keeping Specific Individuals and Creating a New Binary File**  
   This command filters individuals based on a list of IDs, creating a new binary file for these individuals only.

   - **Command**:  
     ```bash
     plink --bfile hapmap-ceu --keep list.txt --make-bed --out selectedIndividuals
     ```
   - **Arguments**:  
     - ***--bfile hapmap-ceu***: Specifies the input binary file.
     - ***--keep list.txt***: Filters individuals to retain only those listed in `list.txt`, where each line contains an individual ID.
     - ***--make-bed***: Specifies that the output should be in binary format.
     - ***--out selectedIndividuals***: Sets the output file prefix to `selectedIndividuals`.

   This command is useful when you need to analyze a subset of individuals, such as those from a specific population or group.

4. **Filtering Individuals Based on Genotyping Rate**  
   This command filters out individuals with a high proportion of missing genotypes, keeping only those with at least 95% genotyping completeness.

   - **Command**:  
     ```bash
     plink --bfile hapmap-ceu --make-bed --mind 0.05 --out highgeno
     ```
   - **Arguments**:  
     - ***--bfile hapmap-ceu***: Specifies the input binary file.
     - ***--make-bed***: Instructs PLINK to produce binary output.
     - ***--mind 0.05***: Filters out individuals with a missing genotype rate greater than 5%.
     - ***--out highgeno***: Specifies `highgeno` as the output file prefix.

   Filtering by genotyping rate ensures data quality by excluding samples with a high degree of missing data.

5. **Selecting a Specific SNP**  
   This command selects a specific SNP and generates a binary file that includes only that SNP.

   - **Command**:  
     ```bash
     plink --bfile hapmap-ceu --snps rs9930506 --make-bed --out rs9930506sample
     ```
   - **Arguments**:  
     - ***--bfile hapmap-ceu***: Specifies the input binary file.
     - ***--snps rs9930506***: Filters the data to retain only the SNP specified by the identifier `rs9930506`.
     - ***--make-bed***: Indicates that the output file should be in binary format.
     - ***--out rs9930506sample***: Sets the prefix for the output file to `rs9930506sample`.

   Selecting specific SNPs is useful when focusing on variants of interest, such as those associated with particular traits or conditions.

---
  

 
 ## Merging Genetic Files and Attaching Phenotypes

### Mergeing Genetic Files
- **Why Merge Genetic Files?**  
  Genetic data is often stored in separate files for each chromosome or different subgroups (e.g., genotype centers or studies). Merging these files into a single dataset is essential for comprehensive analysis. However, care must be taken to handle mismatches and missing data properly.

- **Challenges in Merging**  
  - Variants may differ in availability across files.
  - Variants might have inconsistent alleles or base-pair positions.
  - Differences must be resolved to create a unified dataset.

### Using PLINK for Merging
- **Command:** `--bmerge`  
  The PLINK `--bmerge` command handles mismatches by default by setting such entries to **missing**. Additional options like `--merge-mode` allow customization.

- **Merging Multiple Files:**  
  When merging multiple chromosome-specific files, use the `--merge-list` option with a file containing the names of all files to be merged.

### Example Command
```bash
plink --bfile HapMap_founders --bmerge HapMap_nonfounders --make-bed --out merged_file
```
### Key Options

| **Option**       | **Description**                                                                                         |
|-------------------|---------------------------------------------------------------------------------------------------------|
| `--bfile`         | Specifies the base dataset to start with.                                                              |
| `--bmerge`        | Adds a second dataset to be merged with the base dataset.                                               |
| `--merge-mode`    | Customizes how mismatches are handled during the merge. Default sets mismatches to **missing**.         |
| `--merge-list`    | Merges multiple files listed in a separate text file.                                                   |
| `--make-bed`      | Outputs the merged dataset in binary format (`.bed`, `.bim`, `.fam`).                                   |
| `--out`           | Specifies the name of the output merged dataset.                                                       |


Notes
Ensure all files are compatible before merging (e.g., consistent variant names and formats).
Use --merge-mode for advanced mismatch handling if needed.
This example command demonstrates merging two datasets but can be extended for larger lists using --merge-list.
  
  
  



## Attaching a Phenotype in PLINK

### Overview
The goal of attaching a phenotype in genetic analysis is to study the association between genotype and phenotype. Phenotypic data can be incorporated into genetic files using PLINK's `--pheno` command.

### Input Files
1. **.fam file**  
   Contains family and individual IDs along with phenotype information. Missing phenotypes are denoted by `-9`.  
   Example:  
   ```plaintext
   0 HGO0096 0 0 1 -9
   0 HGO0097 0 0 2 -9
   ```

2. **.bim file**  
   Contains variant information, including chromosome, SNP ID, position, and alleles.  
   Example:  
   ```plaintext
   1 rs1048488 0 760912 G A
   1 rs2519031 0 793947 G A
   ```

3. **Phenotype file (e.g., `BMI_pheno.txt`)**  
   A separate file with phenotypic values for each individual. It typically includes family ID (`FID`), individual ID (`IID`), and the phenotype (e.g., BMI).  
   Example:  
   ```plaintext
   FID IID BMI
   0 HGO0096 25.0228
   0 HGO0097 24.8536
   ```

### PLINK Command to Attach Phenotype
To attach a phenotype to the genetic data:  
```bash
./plink --bfile 1kg_EU_qc --pheno BMI_pheno.txt --make-bed --out 1kg_EU_BMI
```

### Output
1. **Updated .fam File**  
   The last column now contains the phenotypic values instead of `-9` (missing).  
   Example:  
   ```plaintext
   0 HGO0096 0 0 1 25.0228
   0 HGO0097 0 0 2 24.8536
   0 HGO0099 0 0 2 23.6893
   ```
2. **Unchanged .bim File**  
   The variant information remains the same.

### Key Notes
- This approach integrates continuous (quantitative) phenotypes.
- Missing phenotype values are replaced with actual data from the phenotype file.
- The `--make-bed` command creates updated PLINK binary files.


## Further reading
PLINK is a widely-used toolset for analyzing genetic data, especially in population genetics and genome-wide association studies. Common uses of PLINK include:

- Converting between file formats for different types of genotype data.
- Filtering individuals and SNPs based on quality control metrics, such as genotyping rate.
- Performing specific queries on individual variants or groups of individuals.


## Descriptive Statistics

## Allele Frequency Analysis in PLINK

### Overview
Allele frequencies can be calculated using the `--freq` command in PLINK. The output file (with a `.frq` suffix) contains essential information about the alleles for each SNP, including:
- Minor Allele Frequency (MAF)
- Allele codes
- Chromosome and SNP identifiers

### Example Command
```bash
./plink --bfile hapmap-ceu --freq --out Allele_Frequency
```
**Output File (Allele_Frequency.frq)**

The output file includes the following columns:  
  
| Column   | Description                                                                           |
|----------|---------------------------------------------------------------------------------------|
| CHR      | Chromosome number or code for sex chromosomes                                        |
| SNP      | Variant name (e.g., rsID for SNPs)                                                   |
| A1       | Allele 1 (usually the minor allele, i.e., the one with lower frequency)              |
| A2       | Allele 2 (usually the major allele)                                                  |
| MAF      | Minor Allele Frequency (frequency of A1)                                             |
| NCHROBS  | Number of allele observations                                                        |

### Example Output
``` 
CHR    SNP          A1    A2    MAF      NCHROBS
1      rs9629043    G     A     0.05938  116
1      rs11497407   A     G     0.00833  120
1      rs12565286   G     C     0.06780  118
1      rs11804171   A     T     0.03704  108
1      rs2977670    G     C     0.07143  112
```

- The Number of Allele Observations (NCHROBS) represents the total number of alleles observed for a specific genetic variant across all individuals in the dataset.


This value is calculated as:

NCHROBS=2×number of individuals with non-missing genotype data for the SNP
Each individual has two alleles (one from each parent) at a given genetic locus. For example:

if there are 50 individuals with genotype data for a SNP, the NCHROBS will be 
50×2=100.
If some individuals have missing genotype data, the number of observations will decrease accordingly.

### Additional Features
- Use the --within option to stratify allele frequency calculations by a categorical variable.

```bash
./plink --bfile hapmap-ceu --freq --within groups.txt --out Stratified_Frequency
```

### Notes
Minor Allele Frequency (MAF) refers to the frequency at which the second most common allele occurs in a given population. Essentially, it's a measure of how common the less frequent allele is among all the alleles for a particular genetic variant (usually a single nucleotide polymorphism, or SNP)1.

For example, if a SNP has two alleles, A and G, and allele A is found in 70% of the population while allele G is found in 30%, the MAF would be 0.30 (or 30%)
This analysis is helpful for understanding genetic variation across a population.


### Missing Values in PLINK
PLINK allows the examination of missing values to assess data quality for individuals and variants using the --missing command. This produces two output files:

- .imiss: Contains missing information for each individual.
- .lmiss: Contains missing information for each variant.

### Key Command:

```bash
./plink --bfile hapmap-ceu --missing --out missing_data
```

.imiss File Format

- Description: Reports missing genotype data per individual.
- Columns:
    - FID: Family ID.
    - IID: Individual ID.
    - MISS_PHENO: Missing phenotype indicator (Yes/No).
    - N_MISS: Number of missing genotype calls.
    - N_GENO: Total number of potential genotype calls.
    - F_MISS: Missing genotype call rate.
    
 **Example Data:**
 
 | FID  | IID      | MISS_PHENO | N_MISS | N_GENO   | F_MISS  |
|------|----------|------------|--------|----------|---------|
| 1334 | NA12144  | N          | 15077  | 2239392  | 0.006733 |
| 1340 | NA06994  | N          | 16080  | 2239392  | 0.007181 |
| 1340 | NA07022  | N          | 17467  | 2239392  | 0.007800 |


.lmiss File Format

- Description: Reports missing genotype data per variant.
- Columns:
    - CHR: Chromosome code.
    - SNP: Variant identifier (e.g., rsID).
    - N_MISS: Number of missing genotype calls.
    - N_GENO: Total number of valid genotype calls.
    - F_MISS: Missing genotype call rate.

**Example Data:**

| CHR | SNP        | N_MISS | N_GENO | F_MISS  |
|-----|------------|--------|--------|---------|
| 1   | rs12565286 | 1      | 60     | 0.01667 |
| 1   | rs3094315  | 2      | 60     | 0.03333 |
| 1   | rs12138618 | 0      | 60     | 0.00000 |

**Filters in PLINK**

PLINK offers a variety of filters to extract specific subsets of individuals or variants from genetic data.

**Key Filters:**

| Filter Command       | Description                                   |
|-----------------------|-----------------------------------------------|
| `--filter-controls`   | Filters individuals with a binary phenotype marked as controls. |
| `--filter-males`      | Retains only male individuals.               |
| `--filter-females`    | Retains only female individuals.             |
| `--filter-founders`   | Keeps only founder individuals (no known parents). |
| `--filter-nonfounders`| Keeps non-founder individuals.               |

**Example Command:**
```bash
./plink --bfile hapmap-ceu --filter-females --make-bed --out hapmap_filter_females
```


