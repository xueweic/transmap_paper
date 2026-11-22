# TransMap Manuscript Resources

Code and data to reproduce figures in TransMap manuscript.


## About TransMap

TransMap is a generalizable computational framework that combines multi-scale *trans*-QTL mapping with multi-phenotype genetic colocalization to improve the discovery and interpretation of distal regulatory variants. Beyond individual *trans*-eGenes (TransMap-eGene), TransMap constructs local co-regulatory program phenotypes that account for background expression variation (TransMap-local) and genome-wide coordinated programs (TransMap-global), improving the detection of weak, coordinated distal genetic effects. Applied to bulk and single-nucleus transcriptomic profiles and proteomic, glycoproteomic and metabolomic biomarkers from the aging human brain, TransMap provides a multi-scale, cell-type-resolved map of *trans*-regulatory variation.

## Repository Structure

This repository contains the codes and data used to generate figures from our manuscript, available at: https://github.com/xueweic/transmap_paper

- **Simulation Studies**: This directory contains simulation designs and implementation codes used in the paper.

- **Data Applications**: This section contains the data applications discussed in the TransMap paper.

- **Main Figures**: Each notebook is fully executable and documented to ensure reproducibility of our results.

- **Supplementary Figures**: Each notebook is fully executable and documented to ensure reproducibility of our results shown in Supplementary Figures.

Each figure folder contains one notebook per panel and a `data` folder with the plot-ready numerical results (`*.rds`) read by that notebook. Only summary-level results are included; no individual-level data. Provenance and checksums of every `*.rds` file are listed in `data/manifest.tsv`.


## Getting Started

To navigate this resource, use the table of contents in the left sidebar. Each figure section contains interactive notebooks that allow you to:

1. View the code used to generate analyses
2. Examine data associated with the figures
3. Reproduce visualizations

## Computational Requirements

The analyses in this book were performed using:
- R version 4.4 or higher
- Key R packages: ggplot2, patchwork, ggrepel, scales, ComplexUpset, ggtext, ggalluvial

## TransMap Tutorial Website

The TransMap R package is available at [https://github.com/xueweic/transmap](https://github.com/xueweic/transmap), with documentation at [https://xueweic.github.io/transmap/](https://xueweic.github.io/transmap/).

## Citation

If you use TransMap in your research, please cite:

> Cao X, Wang Y, Sun H, Feng R, Zhang H, Li Y, Lee H, Karaahmet B, Li B, Gersbach C, Crawford G, Taga M, Beeri MS, Buchman AS, Klein HU, de Jager PL, Bennett D, The Alzheimer's Disease Functional Genomics Consortium, Wang G, Dey KK. (2026+). A multi-scale cell-type resolved map of *trans*-regulatory architecture in the aging human brain. medRxiv. [https://doi.org/10.64898/2026.11.05.26367384](https://doi.org/10.64898/2026.11.05.26367384)
