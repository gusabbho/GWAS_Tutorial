# Quality Control (per Marker)

## Overview
Per-marker QC focuses on the quality assessment of genetic variants to reduce bias and improve the accuracy of genetic studies. This involves several sequential steps to identify and exclude low-quality markers.

## Steps in Per-Marker QC

1. **Exclusion of Low Call Rate SNPs**  
   - Remove SNPs with poor genotyping performance.

2. **Removal of Rare Variants**  
   - Exclude SNPs with very low allele frequencies for certain analyses.

3. **Hardy-Weinberg Equilibrium (HWE) Check**  
   - Identify and exclude SNPs with extreme deviations from HWE.

4. **Call Rate Comparison in Case-Control Studies**  
   - Remove SNPs with significantly different call rates between case and control groups.

5. **Exclusion of Low-Quality Imputed SNPs**  
   - Discard variants with poor imputation quality.

## Importance
- Removing suboptimal markers minimizes the risk of false positives in genetic analysis.
- Careful consideration of excluded markers is essential, especially regarding linkage disequilibrium (LD) patterns.

This process ensures the reliability and accuracy of genetic study results.


## Quality Control for SNPs: Low Call Rate and Minor Allele Frequency (MAF)

### Low Call Rate SNPs
- **Definition**: SNPs with high genotype missingness, i.e., missing in a large proportion of individuals.  
- **Threshold**: Markers with a call rate < 95% (or missingness > 5%) are excluded.  
- **PLINK Command**: Use the `--geno` option to specify a missing cut-off, e.g., 0.05 for a 5% missing rate.
  ```bash
  ./plink --bfile 1kg_hm3 --geno 0.05 --make-bed --out 1kg_hm3_geno

Output: Creates the following files:

.bim .fam .bed .log

### SNPs with Low Minor Allele Frequency (MAF)
- Definition: Minor Allele Frequency (MAF) is the frequency of the less common allele in a population.

- Threshold:
    - SNPs with a MAF < 1% (0.01) are usually excluded due to:

        1. Low Statistical Power: Insufficient power to detect true
 SNP-trait associations.
 
        2. Genotyping Errors: Rare SNPs are more prone to bias and errors.
- PLINK Command: Use the --maf option to specify the threshold, e.g., 0.01.

```
plink --bfile 1kg_hm3 --maf 0.01 --make-bed --out 1kg_hm3_maf
```

- Threshold Recommendations:
    -Large Sample Size (N ≥ 100,000): MAF ≥ 0.01.
    
    - Moderate Sample Size (N ≈ 10,000): MAF ≥ 0.05.
    
    - Small Sample Size (N < 10,000): Higher MAF thresholds may be applied.

- Advances: The 1-2% MAF threshold has been decreasing with improvements in imputation technologies.

This QC process ensures the exclusion of unreliable SNPs, reducing bias and improving the reliability of genetic association studies.





# Deviation from Hardy-Weinberg Equilibrium (HWE)

## Overview of HWE
- **Assumptions**: 
  - Infinitely large population.
  - No selection, mutation, or migration.
  - Genotype and allele frequencies remain constant across generations.
- **Significance of Deviation**: 
  - Indicates potential genotyping errors, evolutionary forces, or deviations between allele and genotype frequencies.

## HWE in GWAS
- **Expected Genotype Frequencies**: Calculated using allele frequencies.  
  Example: For allele A (0.20) and allele T (0.80), the expected AT genotype frequency is:  
  ( 2 * 0.2 * 0.8 = 0.32) .  
- **Common Cause of Deviation**: Genotyping errors are often assumed to be the primary reason for deviation in GWAS.

## HWE Testing in PLINK
- **Command**: Use the `--hwe` option with a significance threshold. Example:
  ```bash
  ./plink --bfile 1kg_hm3 --hwe 0.00001 --make-bed --out 1kg_hm3_hwe
- Output Files: Generates .log, .bim, .fam, and .bed files.






### Thresholds for Exclusion

- **Binary Traits**:
  - HWE p-value < 10<sup>-6</sup> in cases.
  - HWE p-value < 10<sup>-4</sup> in controls.
  
- **Quantitative Traits**:
  - HWE p-value < 10<sup>-4</sup>.

- **Note**: Thresholds can vary in the literature depending on study requirements and objectives.



### Combining QC Filters

To exclude SNPs failing multiple QC criteria, apply combined filters.

Example PLINK command:
```bash
plink --bfile 1kg_hm3 \
--mind 0.03 \
--geno 0.05 \
--maf 0.01 \
--hwe 0.00001 \
--exclude individuals_failQC.txt \
--make-bed --out 1kg_hm3_QC
```

Filters Applied:
- Individual-level QC: e.g., extreme heterozygosity or related individuals.
- Marker-level QC: Low call rate, low MAF, HWE deviations.

This ensures the removal of low-quality SNPs and individuals for more reliable analysis.







## 8.5.3 Genome-wide Association Meta-Analysis QC

Genome-wide association studies (GWASs) often rely on meta-analyses of association results from multiple datasets. To prevent type II errors (false positives), researchers apply rigorous quality control (QC) at the meta-analysis stage to identify and remove low-quality data. Below are the main steps and filters commonly applied.

### Reasons for QC Repetition
1. **Pooled Cohort Complexity**: Variants that are acceptable in a single study may show issues when combined across multiple cohorts.
2. **Large-Scale Analysis**: GWAS meta-analyses often involve numerous genetic studies worldwide. For example, a 2016 GWAS on human reproductive behavior analyzed sex-specific phenotypes across 63 cohorts, requiring rigorous QC checks.
3. **Independent Validation**: QC is often performed by multiple teams to ensure consistency and accuracy in reported results.

### QC Filters for Meta-Analysis
1. **Harmonization**: Align base pair positions of markers across all datasets using a common reference genome (e.g., NCBI build 37).
2. **Evaluation of Missing or Implausible Data**:
   - Exclude markers with missing information on effect alleles or alternative alleles.
   - Remove markers with implausible effect estimates, standard errors, or p-values.
3. **Exclusion of Problematic Markers**:
   - Remove non-biallelic markers and monomorphic markers (those showing no variation).
4. **Rare Variant Filtering**:
   - Exclude markers with minor allele frequency (MAF) below 1%.
   - For smaller studies, variants with MAF below 5% may also be excluded.
   - Large studies or biobanks can analyze less common variants with confidence.
5. **Imputation Quality**:
   - Drop markers with imputation quality scores below 0.7 to ensure reliable association results.

### Importance of Rigorous QC
GWAS meta-analyses involve complex datasets, often requiring months of careful QC. A conservative approach ensures high-quality results by filtering out problematic markers, even at the risk of excluding potential "true" findings.










## Diagnostic Checks for Genome-Wide Association Meta-Analysis QC

After applying filters during GWAS meta-analysis QC, additional diagnostic checks are performed to ensure data quality. These checks focus on identifying errors in allele frequencies, strand orientations, and heterogeneity across studies.

### Key Diagnostic Checks

#### 1. **Allele Frequency Plots**
- **Purpose**: 
  - Verify uniform coding of variants across studies.
  - Detect errors in allele frequencies and strand orientations.
  - Compare allele frequencies across studies and with a reference dataset (e.g., 1000 Genomes).
- **Visualization**:
  - Scatterplots where:
    - **X-axis**: Reference allele frequencies.
    - **Y-axis**: Study allele frequencies.
  - Variants diverging by more than 0.2 in allele frequency from the reference are highlighted.

#### 2. **Forest Plots**
- **Purpose**: Examine heterogeneity in association results across datasets for a specific variant.
- **Features**:
  - Confidence intervals of association results for each dataset.
  - Larger studies display smaller confidence intervals and more precise estimates.
  - Box size represents sample size.
- **Use**: Ensure all results align in the same direction of effect and evaluate heterogeneity visually.

#### 3. **PZ Plots**
- **Purpose**: Compare reported p-values to Z-score-derived p-values to validate results.

#### 4. **Quantile-Quantile (Q-Q) Plots**
- **Purpose**:
  - Compare observed p-values against theoretical quantiles under the null hypothesis.
  - Identify unaccounted population stratification or inflation due to well-powered GWAS of heritable traits.

#### 5. **Batch Effects**
- **Definition**: Measurement errors caused by variations in laboratory conditions, reagent lots, or processing batches.
- **Reference**: For more on batch effects, see Leek et al., 2010.

### Summary
Diagnostic checks such as allele frequency plots, forest plots, PZ plots, and Q-Q plots help ensure robust and reliable results in GWAS meta-analyses. Addressing batch effects and heterogeneity further enhances the integrity of the study.
