## Data sources

**PubMed**: The **SM**art **A**utomatic **C**lassification method (SMAC) has been implemented for identifying and selecting publications of interest from PubMed. [Gene Expression Omnibus](https://www.ncbi.nlm.nih.gov/geo/) is used to establish computational links between the literature and any associated data. If data is available in the public domain, the system accesses the repository and downloads the relevant data files. These are fed into the relevant analytical pipelines and made available from the PubMed tab.

**TCGA**: The TCGA consortium is dedicated to the systematic study of alterations in a variety of human cancers. It has made publicly available DNA copy number, mRNA expression, methylation and mutation data, alongside its associated clinical data for a range of cancer types/subtypes. Currently, mRNA expression and mutation data are available for analysis.
**CCLE**: DNA copy number, mRNA expression and mutation data for pancreatic cancer cell lines are available from this tab.
**GENIE**: The GENIE dataset represent various types of pancreatic tumours as well as pancreatic precursor lesions and benign conditions. It contains important data regarding DNA copy number alterations, gene fusion and mutations.
**ICGC**: Something about ICGC
**PCRFTB Returned datasets**:

For TCGA, CCLE and ICGC datasets that contain data generated from different technologies, [an integrative modality](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/data_integration.md) is made available to investigate relationships between mutations, expression and copy number changes at individual gene or whole-genome level.

Results are presented in an interactive and informative graphical format using the open source visualization library [Plotly](https://plot.ly). The various statistical and scientific charts allow users to visualise the annotation of data points, zoom to focus on area of interest, exclude/include subgroups in the data, and download as static image files of publication quality.

![statistics](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/statistics.png)
