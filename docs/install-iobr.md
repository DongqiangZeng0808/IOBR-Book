
# **How to install IOBR**

## ⏳ Installing Dependency Packages

It is essential that you have R 3.6.3 or above already installed on your computer or server. IOBR is a pipeline that utilizes many other R packages that are currently available from CRAN, Bioconductor and GitHub.


```r
if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
depens<-c('tibble', 'survival', 'survminer', 'limma', "DESeq2","devtools", 'limSolve', 'GSVA', 'e1071', 'preprocessCore', 
          "devtools", "tidyHeatmap", "caret", "glmnet", "ppcor",  "timeROC", "pracma", "factoextra", 
          "FactoMineR", "WGCNA", "patchwork", 'ggplot2', "biomaRt", 'ggpubr')
for(i in 1:length(depens)){
  depen<-depens[i]
  if (!requireNamespace(depen, quietly = TRUE))  BiocManager::install(depen,update = FALSE)
}
```

## 💻 Install IOBR package

When the dependent environments are built, users are able to install IOBR from github by typing the following code into your R session:

```r
if (!requireNamespace("IOBR", quietly = TRUE))  devtools::install_github("IOBR/IOBR")
```

```
## Warning: package 'tidyHeatmap' was built under R version 4.2.3
```

```r
library(IOBR)
```

```
## Warning: package 'tibble' was built under R version 4.2.3
```

```
## Warning: package 'dplyr' was built under R version 4.2.3
```

```
## Warning: package 'ggplot2' was built under R version 4.2.3
```


## 🔍 The main pipeline of IOBR

<div class="figure" style="text-align: center">
<img src="./fig/IOBR-Package.png" alt="The main pipeline of IOBR" width="95%" />
<p class="caption">(\#fig:flowchart)The main pipeline of IOBR</p>
</div>

## Main Functions

* <div style="color:green">**Data Preparation: data annotation and transformation**</div> 
   * `count2tpm()`: transform count data of RNA sequencing into TPM data.
   * `anno_eset()`: annotate the normalized genes expression matrix, including RNAseq and array (Affymetrix or Illumina).
   * `remove_duplicate_genes()`: remove the genes annotated with the duplicated symbol after normalization and retain only the symbol with highest expression level.
</br>

* <div style="color:green">**TME Deconvolution Module: integrate multiple algorithms to decode immune contexture**</div> 
   * `deconvo_tme()`: decode the TME infiltration with different deconvolution methodologies, based on bulk RNAseq, microarray or single cell RNAseq data.
   * `generateRef()`: generate a novel gene reference matrix for a specific feature such as infiltrating cell, through the  SVR and lsei algorithm.
</br>

* <div style="color:green">**Signature Module: calculate signature scores, estimate phenotype related signatures and corresponding genes, and evaluate signatures generated from single-cell RNA sequencing data **</div>
  * `calculate_sig_score()`: estimate the interested signatures enrolled in IOBR R package, which involves TME-associated, tumor-metabolism, and tumor-intrinsic signatures.
  * `feature_manipulation()`: manipulate features including the cell fraction and signatures generated from multi-omics data for latter analysis and model construction. Remove missing values, outliers and variables without significant variance.
  * `format_signatures()`: generate the object of `calculate_sig_score()`function, by inputting a data frame with signatures as column names of corresponding gene sets, and return a list contain the signature information for calculating multiple signature scores.
  * `format_msigdb()`: transform the signature gene sets data  with gmt format, which is not included in the signature collection and might be downloaded in the MSgiDB website, into the object of `calculate_sig_score()`function.
</br>
    
* <div style="color:green">**Batch Analysis and Visualization: batch survival analysis and batch correlation analysis and other batch statistical analyses **</div>
  * `batch_surv`: batch survival analysis of multiple continuous variables including varied signature scores.
  * `subgroup_survival`: batch survival analysis of multiple categorized variables with different number of subgroups. 
  * `batch_cor()`: batch analysis of correlation between two continuous variables using Pearson correlation coefficient or Spearman's rank correlation coefficient .
  * `batch_wilcoxon()`: conduct batch wilcoxon analyses of binary variables.
  * `batch_pcc()`: batch analyses of Partial Correlation coefficient(PCC) between continuous variables  and minimize the interference derived from confounding factors.
  * `iobr_cor_plot()`: visualization of batch correlation analysis of signatures from 'sig_group'. Visualize the correlation between signature or phenotype with  expression of gene sets  in target signature is also supported.
  * `cell_bar_plot()`: batch visualization of TME cell fraction, supporting input of deconvolution results from 'CIBERSORT', 'EPIC' and 'quanTIseq' methodologies to further compare the TME cell distributions within one sample or among different samples. 
</br>

* <div style="color:green">**Signature Associated Mutation Module: identify and analyze mutations relevant to targeted signatures**</div>
  * `make_mut_matrix()`: transform the mutation data with MAF format(contain the columns of gene ID and the corresponding gene alterations which including SNP, indel and frameshift) into a mutation matrix in a suitable manner for further investigating signature relevant mutations.
  * `find_mutations()`: identify mutations associated with a distinct phenotype or signature.
</br>

* <div style="color:green">**Model Construction Module: feature selection and fast model construct to predict clinical phenotype**</div>
  * `BinomialModel()`: select features and construct a model to predict a binary phenotype.
  * `PrognosticMode()`: select features and construct a model to predict clinical survival outcome.
</br>

