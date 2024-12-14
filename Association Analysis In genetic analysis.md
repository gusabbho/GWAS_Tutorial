# Association Analysis In genetic analysis

A primary goal is to estimate the association between a genotype and a phenotype. The example provided demonstrates how to estimate the linear association of the rs9674439 SNP alleles on body mass index (BMI) using an additive model. In this model, each copy of the C allele (the first allele in the .bim file) has the same effect on the phenotype.

Steps in the Analysis:
Use the PLINK tool with the following command:

1. Use the PLINK tool with the following command:

```bash
plink --bfile 1kg_EUBMI --snps rs9674439 --assoc --linear --out BMIrs9674439
```

* --bfile: Specifies the binary fileset (e.g., 1kg_EUBMI) containing .bed, .bim, and .fam files.
* --snps: Specifies the SNP to analyze, here rs9674439. A range of SNPs can also be used.
* --assoc: Requests basic association analysis results.
* --linear: Specifies a linear regression for quantitative traits.
* --out: Names the output file (e.g., BMIrs9674439.assoc.linear).

**Output Details:**

| CHR | SNP       | BP       | A1 | TEST | NMISS | BETA   | STAT  | P      |
|-----|-----------|----------|----|------|-------|--------|-------|--------|
| 16  | rs9674439 | 33836510 | C  | ADD  | 379  | 0.2974 | 1.269 | 0.2205 |


The output file contains the following fields:

* CHR: Chromosome number.
* SNP: Variant identifier.
* BP: Base-pair position.
* A1: Effect allele.
* TEST: Type of statistical test (e.g., ADD for additive model).
* NMISS: Number of missing values.
* BETA: Regression coefficient.
* STAT: T-statistic.
* P: Asymptotic p-value.

**Example Results:**

For SNP rs9674439:

* Each copy of the C allele reduces BMI by 0.29 units (BETA = -0.2974).
* The result is not statistically significant (p-value = 0.20).


## Summary of Logistic Regression Analysis

The output of a logistic regression differs from that of a linear regression, as it provides an estimate of the odds ratio (OR). Logistic regression analysis is used when the phenotype is dichotomous (e.g., overweight vs. not overweight). Below is an example using rs9674439.  
  
**Output File Details:**

The logistic regression output file includes the following columns:


* CHR: Chromosome number.
* SNP: Variant identifier.
* BP: Base-pair position.
* A1: Effect allele.
* TEST: Type of statistical test (e.g., ADD for additive model).
* NMISS: Number of missing values.
* OR: Odds ratio.
* STAT: T-statistic.
* P: Asymptotic p-value

Interpretation of Odds Ratios:

* OR > 1: Increased risk associated with the allele.
* OR < 1: Decreased risk associated with the allele.
* OR = 1: No association.

**Example Results:**

For SNP rs9674439:


| CHR | SNP       | BP       | A1 | TEST | NMISS | OR  | STAT  | P        |
|-----|-----------|----------|----|------|-------|-----|-------|----------|
| 16  | rs9674439 | 33836510 | C  | ADD  | 1092  | 0.7 | 5.323 | 0.0009   |


* Odds Ratio (OR): 0.7, indicating that the C allele is associated with a reduced risk of being overweight.
* P-value: 0.0009, indicating strong statistical significance.
* Interpretation: The C allele on rs9674439 is protective against becoming overweight.

This result aligns with the findings from the linear regression, though the statistical significance differs because the trait (BMI) is coded differently:
* BMI: Continuous variable.
* Overweight: Dichotomous variable.

**The choice of variable coding depends on the study design and research question.**

## More interpretation for the result:

**Explanation of NMISS Difference**

The NMISS (number of missing values) differs between the linear and logistic regression analyses because the two models handle missing data differently. Specifically:

1. Linear Regression:

    * Analyzes the continuous phenotype (e.g., BMI).
    * Excludes individuals with missing values for either the phenotype or genotype being analyzed.
   
2. Logistic Regression

    * Analyzes a dichotomous phenotype (e.g., overweight vs. not overweight).
    * The dichotomous phenotype requires converting continuous BMI into a binary variable (e.g., overweight = 1, not overweight = 0).
    * If some individuals cannot be classified (e.g., missing BMI or ambiguous binary coding), they are excluded, which may result in a different number of missing values compared to the linear regression.

Thus, the different phenotype definitions and the handling of missing data cause the discrepancy in NMISS.

### Explanation of Odds Ratio (OR)

The **odds ratio (OR)** is a measure of association used in logistic regression. It quantifies how the presence of a particular allele (e.g., the C allele) changes the odds of an outcome (e.g., being overweight).


#### Odds
**Odds:** The ratio of the probability of an event occurring to the probability of it not occurring.

$$
Odds = \frac{\text{P(event)}}{1 - \text{P(event)}}
$$


Where:
- P(event) is the probability of the event occurring.


**Odds Ratio (OR)**

The odds ratio compares the odds of an event between two groups:

$$
OR = \frac{\text{Odds (with allele)}}{\text{Odds (without allele)}}
$$


**Interpretation of OR:**
- **OR > 1**: The allele increases the odds of the event (positive association).
- **OR < 1**: The allele decreases the odds of the event (protective effect).
- **OR = 1**: The allele has no effect on the odds of the event.

**In this example:**

OR = 0.7 means that each copy of the C allele decreases the odds of being overweight by 30% (calculated as 
1−0.7 = 0.3
1−0.7=0.3).


## Additive, Dominant, and Recessive Models in Genotype-Phenotype Analysis
Overview

While additive models are the most common type of genotype-phenotype association analysis, dominant and recessive models are sometimes preferred for studying the effect of specific alleles. These models analyze the impact of genotypes differently:

1. Additive Model: Evaluates the incremental effect of each allele.

    Example: AA vs AB vs BB.

2. Dominant Model: Groups heterozygotes and one homozygote genotype together.

    Example: AA+AB vs BB (effect of having at least one copy of the risk allele A).

3. Recessive Model: Studies the effect of having two copies of the risk allele.

    Example: AA vs AB+BB.
    
 **Regression Models and Commands**
 
 PLINK allows estimation of these models using the following commands:

| Model     | Continuous Outcome | Binary Outcome       | Interpretation |
|-----------|--------------------|----------------------|----------------|
| Additive  | --linear           | --logistic           | AA vs AB vs BB |
| Dominant  | --linear dominant  | --logistic dominant  | AA+AB vs BB    |
| Recessive | --linear recessive | --logistic recessive | AA vs AB+BB    |


Example PLINK Command (Dominant Model):

```
plink --bfile 1kg_EU_BMI --snps rs9674439 --assoc --linear dominant --out BMIrs9674439
```
**Example Output Files**

The command above generates the following files:

1. BMIrs9674439.log: Logs the command execution.
2. BMIrs9674439.assoc_linear: Contains results of the linear association analysis.
3. BMIrs9674439.qassoc: Provides additional association statistics.

**Example Results**

1. BMIrs9674439.assoc_linear:

| CHR | SNP       | BP       | A1 | TEST | NMISS | BETA    | STAT   | P      |
|-----|-----------|----------|----|------|-------|---------|--------|--------|
| 16  | rs9674439 | 33836510 | C  | DOM  | 379   | -0.4783 | -1.462 | 0.1445 |

* Interpretation: In the dominant model, each copy of the C allele reduces BMI by 0.4783 units, but the result is not statistically significant (P=0.1445).

2. BMIrs9674439.qassoc:

| CHR | SNP       | BP       | NMISS | BETA    | SE     | R²       | STAT   | P      |
|-----|-----------|----------|-------|---------|--------|----------|--------|--------|
| 16  | rs9674439 | 33836510 | 379   | -0.2974 | 0.2343 | 0.004254 | -1.269 | 0.2052 |

* Interpretation: The R² value (0.004254) indicates a very small proportion of variation explained by this SNP.



# Association Analysis including covariates in PLINK

**Overview**

Association analysis in PLINK may include covariates such as:
- Sex of the respondent
- Birth year
- Controls for population stratification
- Data-specific variables

### Specifying Covariates
- Use the `--covar` option followed by a tab-separated file of covariates.
- Output includes rows with regression estimates for each covariate per marker.

### Genome-Wide Analysis (GWAS)
- To analyze all genetic variants in a genotype file, omit the `--snp` command.
- A typical GWAS command:
```bash
plink --bfile 1kg_EU_BMI --assoc --linear --out BMIgwas
```    
* The output contains millions of rows, each representing a SNP's analysis.

**Example GWAS Output in PLINK**

The following is an excerpt of the typical output file for a GWAS performed in PLINK. Each row represents the results of the association analysis for a single SNP.

| **CHR** | **SNP**        | **BP**   | **A1** | **TEST** | **NMISS** | **BETA** | **STAT** | **P**      |
|---------|----------------|----------|--------|----------|-----------|----------|----------|------------|
| 1       | rs1048488      | 760912   | C      | ADD      | 579       | 0.6031   | 2.0      | 0.03208    |
| 1       | rs3115850      | 761147   | T      | ADD      | 579       | 0.6056   | 2.1      | 0.03343    |
| 1       | rs2519031      | 793947   | G      | ADD      | 579       | 0.9188   | 1.0      | 0.3087     |
| 1       | rs4970383      | 838555   | A      | ADD      | 579       | 0.01473  | 0.50882  | 0.9531     |
| 1       | rs4475691      | 846808   | T      | ADD      | 579       | -0.050   | -2.22    | 0.08223    |
| 1       | rs1806509      | 853954   | C      | ADD      | 579       | -0.1015  | -0.4786  | 0.6325     |
| 1       | rs7537756      | 854250   | G      | ADD      | 579       | -0.1289  | -0.4769  | 0.6337     |
| 1       | rs28576697     | 870645   | C      | ADD      | 579       | 0.1739   | 0.7539   | 0.4514     |
| 1       | rs7523549      | 879317   | T      | ADD      | 579       | 0.116    | 0.82271  | 0.8204     |

## Column Definitions
- **CHR**: Chromosome number
- **SNP**: Variant identifier (RSID)
- **BP**: Base-pair position on the chromosome
- **A1**: Effect allele
- **TEST**: Type of statistical test performed (e.g., ADD for additive model)
- **NMISS**: Number of missing values
- **BETA**: Regression coefficient (effect size)
- **STAT**: T-statistic value
- **P**: Asymptotic p-value for the T-statistic

**Addressing False Positives**

* Standard p-value threshold of 0.05 leads to a high false-positive rate when testing millions of SNPs.
* For GWAS, a stricter threshold of 5 × 10⁻⁸ (0.00000005) is used.
* SNPs in linkage disequilibrium (LD) will often have similar effects and p-values.

**Within-Family Analysis**

* Within-family analysis accounts for population stratification and genetic nurture.
* Differences in genotypes across siblings act as a random experiment.
* PLINK command for family analysis:

```bash
plink --file mydata --qfam --out myresults
```

**Advanced Association Analysis in PLINK**

PLINK supports other analysis types, including:

* Stratified case/control analysis
* Regression using dosage data
* LASSO regression
* Linear mixed model associations









# Linkage Disequilibrium (LD)

Linkage disequilibrium (LD) refers to the non-random association of alleles at different loci and is influenced by various factors such as selection, genetic recombination, mutation rate, genetic drift, population stratification, and genetic linkage. LD results in a correlation structure across SNPs and is crucial for genetic studies.

## Importance of LD
- **Detecting SNP Correlations**: Helps identify SNPs that are independent or correlated.
- **Harmonizing Across Studies**: Useful when the same SNPs are not measured across all studies.
- **Reducing Data Dimensionality**: Redundant SNPs can be excluded for downstream analyses like ancestry estimation or polygenic risk scoring.

## Measures of LD
1. **r² (R-squared)**:  
   - Measures the square of the correlation coefficient between two SNPs.  
   - Ranges from 0 to 1.  
   - Indicates how well one SNP can predict another.  

2. **D’ (D-prime)**:  
   - Measures the recombination probability between two SNPs.  
   - Scaled between 0 and 1.  
   - `D’ = 0`: Complete linkage equilibrium (frequent recombination).  
   - `D’ = 1`: Complete LD (no recombination).

## Interpreting LD
- **High r²**: Indicates a strong correlation between two SNPs (e.g., SNPs can act as proxies for one another).  
- **High D’**: Indicates low recombination between loci and complete LD.  
- r² is preferred when the goal is predictability, while D’ is used for studying recombination patterns.

## Example: Using PLINK for LD Analysis
PLINK can calculate LD metrics (r² and D’) between SNPs using the `--ld` option. For example:

```bash
./plink --bfile hapmap-ceu --ld rs2883059 rs2777888 --out ld_example
```
**Output Example**

| Haplotype | Frequency | Expectation Under LE |
|-----------|-----------|----------------------|
| CA        | 0.0221    | N/A                  |
| TA        | 0.4500    | 0.2400               |
| CG        | 0.466667  | 0.256667             |
| TG        | 0.0508333 | 0.2293333            |

* r² = 0.715909: Indicates a fairly high correlation between the two SNPs.
* D’ = 1: Indicates complete LD (no recombination).





# Interpretation of `ld_example.log` and `ld_example.hh` 

**ld_example.log**

The `ld_example.log` file contains the results of the LD analysis between two SNPs (`rs2883059` and `rs2777888`) conducted using PLINK. Here's an interpretation of the key results:

### General Information
- **RAM Allocation**: 9,908 MB was reserved for the analysis.  
- **Number of Variants**: 2,239,392 variants were loaded from the `.bim` file.  
- **Number of Individuals**: Data includes 60 individuals (30 males and 30 females).  
- **Genotyping Rate**: The total genotyping rate is 99.20%, indicating a high-quality dataset.  
- **Heterozygous Haploid Warning**: 59 heterozygous haploid genotypes are present and treated as missing. This may indicate potential quality issues with some genotypes.

### LD Analysis Results
- **SNP Pair**: The analysis focuses on the SNPs `rs2883059` and `rs2777888`.  
- **LD Metrics**:
  - **R-squared (r²) = 0.715909**: Indicates a strong correlation between these two SNPs, meaning they share a significant amount of information.
  - **D' = 1**: Suggests that there is complete linkage disequilibrium between the two SNPs, i.e., they are co-inherited almost 100% of the time.  

### Haplotype Frequencies
Haplotypes are combinations of alleles at adjacent loci that are inherited together. The following table summarizes the results:

| Haplotype | Frequency | Expectation Under Linkage Equilibrium (LE) |
|-----------|-----------|-------------------------------------------|
| **CA**    | 0         | 0.21                                      |
| **TA**    | 0.45      | 0.24                                      |
| **CG**    | 0.466667  | 0.256667                                  |
| **TG**    | 0.083333  | 0.293333                                  |

- **Haplotype Frequencies**: The observed haplotype frequencies differ slightly from the expectations under linkage equilibrium (LE), indicating some degree of LD between these loci.
- **In-Phase Alleles**: The in-phase alleles (most common pairing) are **CG/TA**.

---

**ld_example.hh**

The `ld_example.hh` file logs details about heterozygous haploid genotypes in the dataset. These entries highlight discrepancies where haploid genotypes (e.g., on X or Y chromosomes) are observed as heterozygous, which is biologically unexpected.

### Key Information
- The file lists problematic genotype entries, where:
  - The first column indicates the family ID.
  - The second column lists the individual ID.
  - The third column specifies the SNP (`rs5982858`).

### Interpretation
- **Potential Issues**: The presence of heterozygous haploid genotypes (e.g., male X chromosome) could stem from:
  - Data quality issues (e.g., genotyping errors).  
  - Misclassification of sex in the `.fam` file.  
- **Next Steps**:
  - Investigate the flagged SNP (`rs5982858`) for potential data errors.
  - Confirm individual sexes in the `.fam` file.
  - Address or filter out problematic entries to improve dataset quality.

---

## Overall Interpretation
The LD analysis (`ld_example.log`) shows a strong correlation between `rs2883059` and `rs2777888`, with high r² and D' values, suggesting that these SNPs are often co-inherited. The haplotype frequencies also confirm a strong LD. However, the presence of heterozygous haploid genotypes (`ld_example.hh`) requires further investigation to ensure the integrity of the dataset before proceeding with downstream analyses.


# Interpret the difference between D' and R<sup>2</sup>

Having a D' score of 1 and an r² score less than 1 is possible due to the way these two measures capture different aspects of linkage disequilibrium (LD):


**D' Explanation**

* D' measures the degree of recombination between two SNPs. A D' score of 1 indicates no historical recombination between the two loci—essentially, the two SNPs are in complete linkage disequilibrium in the sampled population.
* It is a normalized measure, reflecting whether the alleles of two loci are inherited together more frequently than expected under random association. However, it does not take allele frequencies into account.

**r² Explanation**

* r² measures the statistical correlation between two SNPs. It reflects how well one SNP's alleles predict the alleles at the second SNP. Unlike D', r² incorporates allele frequencies into its calculation.
* If allele frequencies of the two SNPs differ significantly, the predictive power (and hence r²) can be less than 1, even if D' = 1.

**Why r² < 1 When D' = 1?**

* Different Allele Frequencies: If the two SNPs have unequal minor allele frequencies (MAFs), r² decreases. For example, if one SNP has a minor allele frequency of 0.4 and the other has 0.1, their correlation is weaker because one SNP's variation only partially overlaps with the other.

* Perfect Co-Inheritance Without Perfect Correlation: D' = 1 implies no recombination, but this doesn't mean the two SNPs always have the same allele distribution or predictive power. For instance:

    * Suppose SNP1 has alleles A/G, and SNP2 has alleles T/C.
    * All occurrences of A at SNP1 are always paired with T at SNP2, but G at SNP1 might pair with T or C in different proportions. This scenario results in a D' = 1 (no recombination) but r² < 1 (imperfect predictability).


**Intuition**

Think of D' as capturing the historical relationship (presence or absence of recombination) and r² as capturing the current strength of association (how well one SNP predicts the other). If allele frequencies differ, r² will naturally drop, even if D' is maximized.

This distinction makes r² more relevant for applications like GWAS, where the goal is to identify SNPs with strong predictive power, while D' is more relevant for understanding recombination and haplotype structure.





**Finding Proxy Genotypes**

* Proxy SNPs in high LD can be identified using reference panels like 1000 Genomes.
* LDlink: An online tool for exploring LD in various population groups.
    * Website: https://ldlink.nci.nih.gov
    * Provides proxy genotypes and LD measures for SNPs of interest.
  
  

#  LD Pruning

LD pruning is a critical step in genetic data analysis, particularly in genome-wide association studies (GWAS) and related analyses. It focuses on reducing redundancy in SNP datasets by removing SNPs that are highly correlated due to linkage disequilibrium (LD). This process ensures that downstream analyses are performed on a set of independent genetic variants, reducing statistical noise and computational burden. Here's an overview of LD pruning and its implementation:

**Key Concepts in LD Pruning**

1. Linkage Disequilibrium (LD): LD refers to the non-random association of alleles at different loci. SNPs within close proximity on the genome often exhibit strong LD, making some redundant for analysis.

2. Pruning Strategy:

    - Correlation Threshold (r²): Two SNPs are considered independent if their pairwise correlation (r²) is below a defined threshold (e.g., 0.2).
    - Distance Threshold: SNPs separated by a distance larger than a set threshold (e.g., 1,000 kilobases) are generally considered independent.
    - Minor Allele Frequency (MAF): SNPs with a minor allele frequency below a certain cutoff (e.g., 1%) may be excluded as low-frequency variants are less informative in many studies.
3. Iterative Process:

    - SNPs are analyzed in windows across the genome.
    - Within each window, the SNP with the highest MAF or another preferred characteristic is retained.
    - Redundant SNPs with high r² are removed.
    
**PLINK Command for LD Pruning**

Example Command:
 
```bash
plink --bfile 1kg_hm3_qc --maf 0.01 --indep-pairwise 50 5 0.2 --out 1kg_hm3_qce_pruned
```
- --bfile: Specifies the input genotype file in binary format.
- --maf 0.01: Filters out SNPs with MAF below 1%.
- --indep-pairwise 50 5 0.2:
    - 50: Size of the sliding window (in SNPs).
    - 5: Step size (number of SNPs to shift the window).
    - 0.2: Maximum r² threshold for retaining SNPs.
    
- --out: Specifies output file prefix.

**Output of LD Pruning**

Output of LD Pruning
Independent SNPs (`*.prune.in`):

1. This file lists SNPs retained after pruning and can be used for downstream analyses.

2. Excluded SNPs (`*.prune.out`): 
This file lists SNPs removed during pruning.

To create a pruned dataset:
```bash
plink --bfile 1kg_hm3_qc --extract 1kg_hm3_qc_pruned.prune.in --make-bed --out 1kg_hm3_pruned
```

**Comparison with LD Clumping**

While LD pruning focuses on removing correlated SNPs based on distance and r², LD clumping selects one representative SNP per LD block using a statistic like the GWAS p-value. Clumping is commonly used for polygenic risk score calculations, where SNPs with the strongest association to the trait are retained.

**Example of LD Clumping**

In clumping, the SNP with the smallest p-value within an LD block is chosen as the representative SNP.

**Conclusion**

LD pruning and clumping are essential preprocessing steps in genetic studies. Pruning ensures a reduced, non-redundant SNP dataset, while clumping focuses on identifying key SNPs for specific analyses like polygenic scores. Using tools like PLINK, these processes can be efficiently implemented for robust genetic data analysis.














# Population Stratification in Genetic Analysis

### Overview
Population stratification is a critical consideration in genetic research, highlighting the genetic diversity across human populations and the methodological approaches to account for these differences.

### Key Concepts

### 1. Genetic Variation Across Populations
- **Genetic Diversity**: Due to human migration, genetic markers vary significantly between populations
- **Allele Frequency Differences**: 
  - A genetic marker common in one population may be rare or absent in another
  - *Example*: SNP rs2777888 
    - G allele is common in African populations
    - A allele is prevalent in Southeast Asian populations

### 2. Principal Components Analysis (PCA)

#### Methodology
- **Primary Purpose**: Identify and adjust for ancestry differences
- **Technical Approach**: 
  - Reduces multidimensional genetic data to fewer variables
  - First principal component explains the most variability
  - Typically uses first 10-20 principal components

#### Applications
- Understand individual ancestry
- Quality control in genetic studies
- Correct population stratification in Genome-Wide Association Studies (GWASs)

### 3. Analytical Capabilities
- Distinguish between:
  - Major ancestral groups
  - Subtle differences within populations

### 4. Alternative Stratification Methods
- **Family Fixed Effect Models**
  - Uses sibling comparisons
  - Eliminates population stratification within families
- **Linear Mixed Models**
  - Accounts for genetic relatedness








### **Estimating Principal Components in PLINK**
- The PLINK command `--pca` calculates principal components for stratification adjustment:
```bash
plink --bfile 1kg_hm3_pruned --pca 10 --out 1kg_pca
```
  
  - Outputs

    - `*.eigenval`: Contains eigenvalues representing the variance explained by each PC.
        - Example:
        
             54.1464
        
             40.0338
         
             6.96377
         
            3.375
    - `*.eigenvec`: Contains eigenvectors (PCs) for individuals, used in further analysis.
        
        - Example:

            0 HGO0096 0.0149253 -0.0329941 0.0157409 0.00171199 ...
            
            0 HGO0097 0.0146554 -0.0330726 0.0168457 -0.00070785 ...

            .
            
            .
            
            .



### R Code for PCA Visualization

The provided R code demonstrates how to visualize principal components (PCs) to inspect population structure in genetic data. Below is a breakdown of the steps:

1. Load Necessary Libraries:

   - The ggplot2 library is used for plotting.

2. Load PCA Data:

   - The PCA output from PLINK (*.eigenvec) is read into R as a table.
    
   - Columns are named, including PC1 (pcal), PC2 (pca2), and subsequent PCs up to PC10.

3. Plot PCs Without Population Labels (Panel A):

    - A scatterplot of PC1 versus PC2 is created using ggplot.
    
    - This plot reveals clustering patterns that indicate population substructures, 
    even without additional labeling.

code:
```r
library(ggplot2)
columns <- c("fid", "Sample.name", "pcal", "pca2", "pca3",
             "pca4", "pca5", "pca6", "pca7", "pca8", "pca9", "pca10")
pca <- read.table(file="lkg_pca.eigenvec", sep="", header=F, col.names=columns)[,c(2:12)]
ggplot(pca, aes(x=pcal, y=pca2)) + 
    geom_point() + 
    theme_bw() + 
    xlab("PC1") + 
    ylab("PC2")
```    


![](./images/PCs_without_population_labels.png)

4. Add Population Labels (Panel B):

    - Information about individuals' populations (from 1kg_samples.txt) is merged with the PCA data.
    - A scatterplot of PC1 versus PC2 is created, with points colored by population (e.g., European, African, East Asian, American ancestry).
    
 Code:
 
 ```r
 geo <- read.table(file="lkg_samples.txt", sep="\t", header=T)[,c(1,4,5,6,7)]
data <- merge(geo, pca, by="Sample.name")
ggplot(data, aes(x=pcal, y=pca2, col=Superpopulation.name)) + 
    geom_point() + 
    theme_bw() + 
    xlab("PC1") + 
    ylab("PC2") + 
    labs(col="")
```


![](./images/PCs_with_population_labels.png)










### **PCA and Advanced Methods to Control Population Stratification**

1. **PCA in Population Stratification**:
   - PCA is effective at distinguishing both large-scale (macro) ancestry differences and subtle variations within homogeneous populations.

   - PCA’s ability to detect population structure makes it a widely used method in GWAS to control for stratification.

2. **Limitations of PCA**:
   - Restricting analyses to a single ancestry (e.g., Europeans) does not entirely eliminate biases from population stratification.
   - It may still fail to account for finer-scale differences within populations.

3. **Family-Based Models**:
   - **Concept**: Uses genetic differences among siblings (e.g., dizygotic twins) to eliminate population stratification biases, as siblings share no stratification but differ due to meiosis.
   - **Advantages**:
     - Provides robust estimates of genetic variant effects without stratification.
   - **Challenges**:
     - Siblings share 50% of genetic variants, reducing variability.
     - Requires larger sample sizes to achieve the same statistical power as unrelated individuals.
     - Limited by smaller family-based datasets compared to large population-based datasets like the UK Biobank.

4. **Linear Mixed Models (LMMs)**:
   - **Concept**: Incorporates a genetic relatedness matrix to adjust for population structure in GWAS.
   - **Advantages**:
     - Accounts for the degree of genetic relatedness across samples.
     - Can replace or complement PCA for stratification control.
   - **Tools**:
     - **BOLT-LMM**: A widely used software implementing LMMs for GWAS.

5. **Applications**:
   - Family fixed effect models and LMMs are often used as robustness checks or alternatives to PCA.
   - These methods enhance stratification control, especially in datasets with complex or subtle population structures.

By integrating PCA, family-based models, and LMMs, geneticists can robustly address population stratification to produce reliable GWAS results.



### IBS Distance Calculation using PLINK

To identify related individuals and reduce bias in association analysis, the **Identity by State (IBS)** metric is used. IBS measures the average proportion of shared alleles between pairs of individuals and is calculated using independent genotyped SNPs. PLINK software provides the functionality to compute IBS distances based on genotyped SNP differences.

#### Command to Calculate IBS Distances
The following command generates a tab-delimited text file containing Hamming distances between individuals:

note: since we hadn't `1kg_samples_EUR.txt` file we first used the following code to make it:  
  
```bash
 grep EUR 1kg_samples.txt |awk '{print 0,$1}' >1kg_samples_EUR.txt
 ```
 next we used the code:

```bash
./plink --bfile 1kg_hm3_pruned --keep 1kg_samples_EUR.txt --distance --out ibs_matrix
```


**Key Features of the Output File:**

1. Output Format:

- The file contains pairwise Hamming distances.
- The first element represents the distance between genome 1   and genome 2.
- Distances involving the same genome (self-distances) are     omitted, as they are 0.
- By default, the output is a lower-triangular matrix.

2. Symmetry:

- The IBS distances are symmetric, meaning the distance between genome 1 and genome 2 is the same as between genome 2 and genome 1.

3. Square Matrix Option:

- To produce a full square matrix, the --distance square option can be used.

Example Output:

```bash
head ibs matrix.dist
```

The first 10 rows of the IBS matrix for the 1kg_samples_EUR.txt dataset look as follows:


74407

74208 74953

74755 74396 73099

74429 74572 73941 73346

74049 74752 74473 74218 74544

74792 74829 74704 75257 74797 74915

75499 74890 74743 74858 74740 74780 75025

74915 74522 74291 75122 74148 74892 73305 73548

74601 74286 74731 74774 74502 74734 75247 75260 74556

73911 74190 73761 74260 74132 74018 74595 74408 74186 74116


***This matrix demonstrates pairwise IBS distances for the first 10 genomes in the sample dataset.***




### Understanding Cryptic Relatedness

The genetic relatedness values can help identify cryptic relatedness between individuals. Expected values for relatedness include:
- **1**: Monozygotic (MZ) twins or duplicated pairs.
- **0.5**: First-degree relatives (e.g., full siblings or parent-offspring).
- **0.25**: Second-degree relatives (e.g., grandparent-grandchild).
- **0.125**: Third-degree relatives (e.g., cousins).

These are theoretical values; actual empirical results may differ. For example, the genetic relatedness between siblings typically averages **0.5**, but ranges from **0.4 to 0.6**.



**Code example:**

```bash
plink --bfile 1kg_hm3_pruned --keep 1kg_samples_EUR.txt --make-rel --out rel_matrix
```



#### Observations from Results
1. **Self-Relatedness**: Genetic relatedness of an individual with themselves differs from 1, reflecting the inbreeding coefficient, which relates to haplotype relationships within an individual.
2. **Higher-Than-Expected Relatedness**:
   - Genome 4 and Genome 3 exhibit a genetic relatedness value higher than expected for unrelated individuals, suggesting they are **second-degree relatives**.
3. **Negative Values**:
   - Some genetic relatedness values are negative due to normalization against the average distance among unrelated individuals. 
   - It is critical to calculate relatedness matrices only within the same ancestry group to ensure accuracy.

**to show the result use the following code.**

```bash
head rel_matrix.rel
```

#### Sample Genetic Relatedness Matrix
Below is an excerpt of the genetic relatedness matrix:

0.975623

0.00309471&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0.980394

0.00433624&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-0.00307206&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0.977987

0.00320043&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0.00425093&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0.0261669&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0.984606



 **Why Do Negative Values Occur?**
 
Negative values are a result of normalization:

* When calculating genetic relatedness, the average distance among unrelated individuals is subtracted to standardize the results.
* If two individuals share less DNA than the typical unrelated pair, their normalized value becomes negative.






### GCTA and Its Applications
**Overview of GCTA**

GCTA (Genome-wide Complex Trait Analysis) is command-based software designed to estimate the genetic relationship of individuals and calculate SNP-based heritability. Originally developed to estimate heritability using genome-wide data, GCTA has since been extended to perform more advanced analyses such as bivariate models and decomposition of genetic variance into multiple parts.

**Basic Applications of GCTA**

- GCTA can decompose phenotypic variance into two components:
    - V(G): Variance explained by additive genetics.
    - V(e): Variance due to environmental (non-genetic) factors.
    
- The heritability estimate h² (V(G)/Vp) represents the proportion of phenotypic variance attributable to genetics. For example, in the provided example for BMI:
    - h² = 0.39, indicating 39% of BMI variance is due to genetic factors.
    - However, with a small sample size (320 individuals) and high standard error (SE = 0.93), the estimate is imprecise, and we fail to reject the null hypothesis of no genetic effect.
    
**Steps to Estimate Heritability Using GCTA**

**1. Calculate a Genomic Relationship Matrix (GRM):**
- The GRM quantifies the genetic relatedness of individuals based on SNP data.

- Example command to calculate GRM:

```bash
gcta64 --bfile 1kg_EU_BMI --autosome --maf 0.01 --make-grm --out 1kg_gcta
```

**Options explained:**

- ***--bfile:*** Specifies the genotype input files (PLINK format).
- ***--autosome:*** Includes only autosomal chromosomes (optional).
- ***--maf 0.01:*** Excludes SNPs with a minor allele frequency below 1% (optional).

- ***--make-grm:*** Creates the GRM.
- ***--out:*** Specifies the name of the output file.

**Output files produced:**

***1. 1kg_gcta.grm.id:*** Contains IDs of individuals.

***2. 1kg_gcta.grm.bin:*** Binary file with the GRM.

***3. 1kg_gcta.grm.N.bin:*** Binary file with sample information.

***4. 1kg_gcta.log:*** Log file.



&nbsp;

**2. Remove Related Individuals:**

- To ensure unrelated individuals are used in the analysis, related individuals (based on a GRM cutoff) are removed.

- Example command

```bash
gcta64 --grm 1kg_gcta --grm-cutoff 0.025 --make-grm --out 1kg_rm025
```
- This creates a new GRM, removing individuals with relatedness greater than 0.025.

**3. Estimate SNP-Based Heritability:**

- To estimate heritability, provide a phenotype file using the --pheno option.

- Example command:

```bash
gcta64 --grm 1kg_rm025 --pheno BMI_pheno.txt --reml --out 1kg_BMI_h2
```
- Key output file:
    - 1kg_BMI_h2.hsq: Contains the summary results of heritability estimation.
    
    
    | Source  | Variance   | Standard Error (SE) |
|---------|------------|---------------------|
| V(G)    | 3.36       | 8.07                |
| V(e)    | 5.26       | 8.06                |
| Vp      | 8.62       | 0.68                |
| V(G)/Vp | 0.39 (39%) | 0.93                |


**Interpretation:**

- Approximately 39% of the variance in BMI is attributed to genetics. However, the SE (0.93) is very high, indicating the estimate is imprecise.
- A log-likelihood ratio test (LRT) was performed to test whether ℎ2≠0, but the resulting p-value (0.364) suggests the null hypothesis cannot be rejected. This means there is no evidence of a significant genetic component for BMI in this dataset.
    
    
**Key Limitations of the Example**

- Sample Size: The dataset contains only 320 individuals, which is too small for reliable heritability estimation.
- Precision: The high SE relative to the point estimate indicates that the result is imprecise and lacks statistical power.
- Conclusion: Although real-world studies estimate the genetic contribution to BMI to be 20-30%, this small dataset does not provide enough evidence to detect it.
    
    
    
    
**Key Notes on Usage**

- GCTA is similar to PLINK in handling genotype data and uses PLINK binary files as input.
- Larger sample sizes are crucial for precise and meaningful estimates of heritability.
- Advanced models can include multiple matrices to dissect genetic variance further.

This workflow illustrates the basic steps to calculate SNP-based heritability using GCTA and highlights the importance of data quality and sufficient sample size in genetic analyses.




























  
  
  
  
  



