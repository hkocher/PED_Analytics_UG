## Copy Number Alterations

The DNA copy number analyses are available for TCGA, GENIE, CCLE and ICGC datasets. This module allows users to analyse and visualise genomic data by querying copy number alteration (CNA) status of selected genes or by exploring frequency of all gain and loss events across all samples.

The CNA status in each sample is defined as deletion (relative linear copy-number value < -2), loss (< -1), gain (> 1), amplification (> 2) or diploid (otherwise). For queried set of genes (at least 2 and up to 20), the portal displays a heatmap with red and blue colours corresponding to gain and loss events, respectively. Grey cells correspond to genes with diploid DNA copy-number. Genes and samples are represented by rows and columns, respectively.

![cna](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/cna.png)

The whole-genome results are visualised in a form of an interactive CIRCOS plot, where each track represents individual genetic events. **The over-expressed (z-score > 1) and under-expressed (z-score < -1) genes are indicated as dark red and dark blue dots on the red and blue circles, respecively.** Users can click on a chromosome to see a detailed view of the chromosome on the UCSC Genome Browser (bottom). Similarly, users can click on any part of a track, which will expand the respective chromosomal region in the UCSC genome browser.

![cna_wg](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/cna_wg.svg)
