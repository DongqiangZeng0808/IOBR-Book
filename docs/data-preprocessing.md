
# **RNA Data preprocessing**
## Loading packages

Load the IOBR package in your R session after the installation is complete:

```r
library(IOBR)
library(tidyverse)
library(clusterProfiler)
```

## Downloading data for example
Obtaining data set from GEO [Gastric cancer: GSE62254](https://pubmed.ncbi.nlm.nih.gov/25894828/) using `GEOquery` R package.

```r
if (!requireNamespace("GEOquery", quietly = TRUE))  BiocManager::install("GEOquery")
library("GEOquery")
# NOTE: This process may take a few minutes which depends on the internet connection speed. Please wait for its completion.
eset_geo<-getGEO(GEO     = "GSE62254", getGPL  = F, destdir = "./")
eset    <-eset_geo[[1]]
eset    <-exprs(eset)
eset[1:5,1:5]
```

```
##           GSM1523727 GSM1523728 GSM1523729 GSM1523744 GSM1523745
## 1007_s_at  3.2176645  3.0624323  3.0279131   2.921683  2.8456013
## 1053_at    2.4050109  2.4394879  2.2442708   2.345916  2.4328582
## 117_at     1.4933412  1.8067380  1.5959665   1.839822  1.8326058
## 121_at     2.1965561  2.2812181  2.1865556   2.258599  2.1874363
## 1255_g_at  0.8698382  0.9502466  0.8125414   1.012860  0.9441993
```

## Gene Annotation

Annotation of genes in the expression matrix and removal of duplicate genes.

```r
# Load the annotation file `anno_hug133plus2` in IOBR.
head(anno_hug133plus2)
```

```
## # A tibble: 6 × 2
##   probe_id  symbol 
##   <fct>     <fct>  
## 1 1007_s_at MIR4640
## 2 1053_at   RFC2   
## 3 117_at    HSPA6  
## 4 121_at    PAX8   
## 5 1255_g_at GUCA1A 
## 6 1294_at   MIR5193
```


```r
# Load the annotation file `anno_grch38` in IOBR.
head(anno_grch38)
```

```
##                id eff_length        gc entrez   symbol chr     start       end
## 1 ENSG00000000003       4536 0.3992504   7105   TSPAN6   X 100627109 100639991
## 2 ENSG00000000005       1476 0.4241192  64102     TNMD   X 100584802 100599885
## 3 ENSG00000000419       9276 0.4252911   8813     DPM1  20  50934867  50958555
## 4 ENSG00000000457       6883 0.4117391  57147    SCYL3   1 169849631 169894267
## 5 ENSG00000000460       5970 0.4298157  55732 C1orf112   1 169662007 169854080
## 6 ENSG00000000938       3382 0.5644589   2268      FGR   1  27612064  27635277
##   strand        biotype
## 1     -1 protein_coding
## 2      1 protein_coding
## 3     -1 protein_coding
## 4     -1 protein_coding
## 5      1 protein_coding
## 6     -1 protein_coding
##                                                                                                  description
## 1                                                          tetraspanin 6 [Source:HGNC Symbol;Acc:HGNC:11858]
## 2                                                            tenomodulin [Source:HGNC Symbol;Acc:HGNC:17757]
## 3 dolichyl-phosphate mannosyltransferase polypeptide 1, catalytic subunit [Source:HGNC Symbol;Acc:HGNC:3005]
## 4                                               SCY1-like, kinase-like 3 [Source:HGNC Symbol;Acc:HGNC:19285]
## 5                                    chromosome 1 open reading frame 112 [Source:HGNC Symbol;Acc:HGNC:25565]
## 6                          FGR proto-oncogene, Src family tyrosine kinase [Source:HGNC Symbol;Acc:HGNC:3697]
```


```r
# Load the annotation file `anno_gc_vm32` in IOBR for mouse RNAseq data
head(anno_gc_vm32)
```

```
##                   id eff_length        gc symbol      mgi_id      gene_type
## 1 ENSMUSG00000000001       3262 0.4350092  Gnai3   MGI:95773 protein_coding
## 2 ENSMUSG00000000003        902 0.3481153   Pbsn MGI:1860484 protein_coding
## 3 ENSMUSG00000000028       3506 0.4962921  Cdc45 MGI:1338073 protein_coding
## 4 ENSMUSG00000000031       2625 0.5588571    H19   MGI:95891         lncRNA
## 5 ENSMUSG00000000037       6397 0.4377052  Scml2 MGI:1340042 protein_coding
## 6 ENSMUSG00000000049       1594 0.5050188   Apoh   MGI:88058 protein_coding
##       start       end transcript_id  ont
## 1 108014596 108053462          <NA> <NA>
## 2  76881507  76897229          <NA> <NA>
## 3  18599197  18630737          <NA> <NA>
## 4 142129262 142131886          <NA> <NA>
## 5 159865521 160041209          <NA> <NA>
## 6 108234180 108305222          <NA> <NA>
```


### For Array data: HGU133PLUS-2 (Affaymetrix)

```r
# Conduct gene annotation using `anno_hug133plus2` file; If identical gene symbols exists, these genes would be ordered by the mean expression levels. The gene symbol with highest mean expression level is selected and remove others. 

eset<-anno_eset(eset       = eset,
                annotation = anno_hug133plus2,
                symbol     = "symbol",
                probe      = "probe_id",
                method     = "mean")
eset[1:5, 1:3]
```

```
##              GSM1523727 GSM1523728 GSM1523729
## SH3KBP1        4.327974   4.316195   4.351425
## RPL41          4.246149   4.246808   4.257940
## EEF1A1         4.293762   4.291038   4.262199
## COX2           4.250288   4.283714   4.270508
## LOC101928826   4.219303   4.219670   4.213252
```

### For RNAseq data

Download RNAseq data using UCSCXenaTools


Transform gene expression matrix into TPM format, and conduct subsequent annotation. 


## Identifying outlier samples

Take ACRG microarray data for example

```r
# source("E:/18-Github/Organization/IOBR/R/find_outlier_samples.R")
res <- find_outlier_samples(eset = eset, project = "ACRG", show_plot = TRUE)
```

<img src="data-preprocessing_files/figure-epub3/unnamed-chunk-9-1.png" style="display: block; margin: auto;" />

```
## [1] "GSM1523817" "GSM1523858" "GSM1523984" "GSM1523988" "GSM1524030"
```

```r
eset1 <- eset[, !colnames(eset)%in%res]
```

## PCA analysis of molecular subtypes


```r
data("pdata_acrg")
res<- iobr_pca(data       = eset1,
              is.matrix   = TRUE,
              scale       = TRUE,
              is.log      = FALSE,
              pdata       = pdata_acrg, 
              id_pdata    = "ID", 
              group       = "Subtype",
              geom.ind    = "point", 
              cols        = "normal",
              palette     = "jama", 
              repel       = FALSE,
              ncp         = 5,
              axes        = c(1, 2),
              addEllipses = TRUE)
```

```
## 
##       CIN       EBV       EMT        GS       MSI MSS/TP53- MSS/TP53+ 
##         0         0        42         0        68       106        79 
## [1] ">>-- colors for PCA: #5f75ae" ">>-- colors for PCA: #64a841"
## [3] ">>-- colors for PCA: #e5486e" ">>-- colors for PCA: #de8e06"
```

```r
res
```

<img src="data-preprocessing_files/figure-epub3/unnamed-chunk-10-1.png" style="display: block; margin: auto;" />


## Batch effect correction

Obtaining another data set from GEO [Gastric cancer: GSE57303](https://www.ncbi.nlm.nih.gov/pubmed/24935174/) using `GEOquery` R package.

```r
# NOTE: This process may take a few minutes which depends on the internet connection speed. Please wait for its completion.
eset_geo<-getGEO(GEO     = "GSE57303", getGPL  = F, destdir = "./")
eset2    <-eset_geo[[1]]
eset2    <-exprs(eset2)
eset2[1:5,1:5]
```

```
##           GSM1379261 GSM1379262 GSM1379263 GSM1379264 GSM1379265
## 1007_s_at    8.34746    9.67994    8.62643    8.59301    8.63046
## 1053_at      5.07972    4.46377    5.29685    5.78983    4.33359
## 117_at       5.65558    4.48732    4.21615    5.47984    5.20816
## 121_at       5.95123    7.09056    6.19903    5.89872    5.91323
## 1255_g_at    1.66923    1.98758    1.73083    1.56687    1.63332
```

Annotation of genes in the expression matrix and removal of duplicate genes.

```r
eset2<-anno_eset(eset       = eset2,
                 annotation = anno_hug133plus2,
                 symbol     = "symbol",
                 probe      = "probe_id",
                 method     = "mean")
eset2[1:5, 1:5]
```

```
##         GSM1379261 GSM1379262 GSM1379263 GSM1379264 GSM1379265
## ND4        13.1695    13.1804    13.0600    12.4544    13.0457
## ATP6       13.1433    13.0814    13.0502    12.4831    13.1168
## SH3KBP1    12.9390    13.1620    12.9773    12.8745    13.1169
## COX2       13.0184    13.0489    12.8621    12.7489    12.9732
## RPL41      13.0201    12.6034    12.7929    13.0153    12.9404
```



```r
eset_com <- remove_batcheffect( eset1       = eset1,  
                                eset2       = eset2,   
                                eset3       = NULL,
                                id_type     = "symbol",
                                data_type   = "array", 
                                cols        = "normal", 
                                palette     = "jama", 
                                log2        = TRUE, 
                                check_eset  = TRUE,
                                adjust_eset = TRUE,
                                repel       = FALSE,
                                path        = "result")
```

```
## 
## eset1 eset2 
##   295    70 
## [1] ">>-- colors for PCA: #5f75ae" ">>-- colors for PCA: #64a841"
## 
## eset1 eset2 
##   295    70 
## [1] ">>-- colors for PCA: #5f75ae" ">>-- colors for PCA: #64a841"
```

<img src="data-preprocessing_files/figure-epub3/unnamed-chunk-13-1.png" style="display: block; margin: auto;" />

```r
dim(eset_com)
```

```
## [1] 21752   365
```

待补充-RNAseq的批次校正：count, combat-seq

## References

Yuqing Zhang and others, ComBat-seq: batch effect adjustment for RNA-seq count data, NAR Genomics and Bioinformatics, Volume 2, Issue 3, September 2020, lqaa078, https://doi.org/10.1093/nargab/lqaa078

Leek, J. T., Johnson, W. E., Parker, H. S., Jaffe, A. E., & Storey, J. D. (2012). The sva package for removing batch effects and other unwanted variation in high-throughput experiments. Bioinformatics, 28(6), 882-883.