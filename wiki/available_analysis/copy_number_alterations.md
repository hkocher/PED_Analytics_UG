The DNA copy number analyses are available for TCGA, GENIE and CCLE datasets. This module allows users to analyse and visualise genomic data by querying copy number alteration (CNA) status of selected genes or by exploring frequency of all gain and loss events across all samples.

The CNA status in each sample is defined as deletion (relative linear copy-number value < -2), loss (< -1), gain (> 1), amplification (> 2) or diploid (otherwise). For queried set of genes (at least 2 and up to 20), the portal displays a heatmap with red and blue colours corresponding to gain and loss events, respectively. Grey cells correspond to genes with diploid DNA copy-number. Genes and samples are represented by rows and columns, respectively.

![cna](https://github.research.its.qmul.ac.uk/hfx320/PED_Analytics/blob/master/images/doc/cna.png)

The frequency plots illustrating the fraction of samples with per-gene gain and loss events (y-axis) in the genomic context (x-axis), are generated for individual groups. The number of samples in each group is indicated in brackets on the top of the plots. Users can highlight regions of possible interest and further zoom in to explore the CNA status of nearby genes.

![cna_chr](https://github.research.its.qmul.ac.uk/hfx320/PED_Analytics/blob/master/images/doc/cna_chr.png)
