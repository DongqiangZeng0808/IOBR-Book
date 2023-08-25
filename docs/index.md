--- 
title: "IOBR (Immuno-Oncology Biological Research)"
author: "Dongqiang Zeng"
date: "2023-08-25"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
url: https://iobr.github.io/book/
cover-image: ./fig/cover.png
geometry:
  - top=1in
  - left=1in
  - right=1in
  - bottom=1in
fontsize: 12pt
linestretch: 1.2
description: "The tutorial of IOBR package"
biblio-style: apalike
csl: chicago-fullnote-bibliography.csl
---
# **Introduction**{-}
Preface
<div class="figure" style="text-align: center">
<img src="./fig/IOBR-Workflow.png" alt="The workflow of IOBR" width="95%" />
<p class="caption">(\#fig:unnamed-chunk-1)The workflow of IOBR</p>
</div>

## 📚 Introduction

IOBR is design for [Immuno-Oncology Biological Research](https://github.com/IOBR/IOBR).
Recent advance in next-generation sequencing has triggered the rapidly accumulating publicly available multi-omics data. The application of integrated omics to exploring robust signatures for clinical translation is increasingly highlighted in immuno-oncology but raises computational and biological challenges. This vignette aims to demonstrate how to utilize the package named IOBR to perform multi-omics immuno-oncology biological research to decode tumor microenvironment and signatures for clinical translation.

This R package integrates 8 published methodologies for decoding tumor microenvironment (TME) contexture: `CIBERSORT`, `TIMER`, `xCell`, `MCPcounter`, `ESITMATE`, `EPIC`, `IPS`, `quanTIseq`. Moreover, 255 published signature gene sets were collected by IOBR, involving tumor microenvironment, tumor metabolism, m6A, exosomes, microsatellite instability, and tertiary lymphoid structure. Run the function `signature_collection_citation` to obtain the source papers, and the function `signature_collection` returns the detail signature genes of all given signatures. Subsequently, IOBR adopts three computational methods to calculate the signature score, comprising `PCA`, `z-score`, and `ssGSEA`. To note, IOBR collected and employed multiple approaches for variable transition, visualization, batch survival analysis, feature selection, and statistical analysis. Batch analysis and visualization of corresponding results are supported. The details of how IOBR works are described below.


## 💿 License

**IOBR** is released under the GPL v3.0 license. See [LICENSE](https://github.com/IOBR/IOBR/blob/master/LICENSE) for details. The code contained in this book is simultaneously available under the [GPL license](https://www.gnu.org/licenses/why-not-lgpl.html); this means that you are free to use it in your packages, as long as you cite the source. The online version of this book is licensed under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.](https://creativecommons.org/licenses/by-nc-sa/4.0/)

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a>


## 🏆 Publishment

Zeng D, Ye Z, Shen R, Yu G, Wu J, Xiong Y,..., Liao W (2021) **IOBR**: Multi-Omics Immuno-Oncology Biological Research to Decode Tumor Microenvironment and Signatures. *Frontiers in Immunology*. 12:687975. [doi: 10.3389/fimmu.2021.687975](https://www.frontiersin.org/articles/10.3389/fimmu.2021.687975/full)


## ✉️ Reporting bugs
Please report bugs to the [Github issues page](https://github.com/IOBR/IOBR/issues)

E-mail any questions to <dongqiangzeng0808@gmail.com>

