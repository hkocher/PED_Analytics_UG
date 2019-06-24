# PED Analytics
This document describes the [**Analytics**](http://www.analytics.pancreasexpression.org) component of **Pancreatic Expression Database** (PED).

This PED component was developed to conduct transcriptomic, genomic and mutational analyses using publicly available data. This includes data obtained from [Gene Expression Omnibus](https://www.ncbi.nlm.nih.gov/geo/) (GEO), [The Cancer Genome Atlas](https://cancergenome.nih.gov/) (TCGA), [Genomics Evidence Neoplasia Information Exchange](http://www.aacr.org/Research/Research/Pages/aacr-project-genie.aspx#.Wrkpy5PwZ25) (GENIE) the [Cancer Cell Line Encyclopedia](https://portals.broadinstitute.org/ccle/home) (CCLE) and the [International Cancer Genome Consortium](https://icgc.org/).

## The server
PED analytics is located on SNPnexus server **138.37.198.15**

Version | Website | directory
------------ | ------------ | ------------
Live | http://www.analytics.pancreasexpression.org | /var/www/html/bioinf
Beta version | http://www.analytics.pancreasexpression.org/beta_version | /var/www/html/bioinf/beta_version

## Structure of the user guide

### Available Analyses

Description | Wiki page
------------ | ------------
Data sources for the analysis | [wiki/available_analysis/Data-sources](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/data_sources.md)|
Principal Component Analysis | [wiki/available_analysis/PCA](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/pca.md)|
Estimates of tumour purity | [wiki/available_analysis/Estimate-tumour-purity](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/tumour_purity.md)|
Gene expression profiles | [wiki/available_analysis/Expression-profile](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/gene_expression.md)|
Evaluation of correlation between genes | [wiki/available_analysis/Correlation](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/correlation.md)|
Survival analysis | [wiki/available_analysis/Survival-analysis](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/survival.md)|
Protein-protein interaction (PPI) networks | [wiki/available_analysis/Gene-networks](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/gene_network.md)|
Copy number alterations | [wiki/available_analysis/Copy-number-alterations](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/copy_number_alterations.md)|
Gene fusion | [wiki/available_analysis/Gene-fusion](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/gene_fusion.md)|
Exploring mutation profiles across samples | [wiki/available_analysis/Mutations](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/mutations.md)|
Integrating Gene expression, copy number and mutations | [wiki/available_analysis/Data-integration](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/available_analysis/data_integration.md)|

### Maintenance of the resource

| Description | Details |
| ------------|---------|
| Transfer the resource to another server/location | [wiki/maintenance/transfer_resource](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/maintenance/transfer_resource.md) |
| Manage an article in the PubMed section | [wiki/maintenance/manage_article](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/maintenance/manage_article.md) |
| Manage a new record in TCGA, CCLE, Genie | [wiki/maintenance/manage_record](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/maintenance/manage_record.md) |
| Manage a study in ICGC | [wiki/maintenance/manage_icgc](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/maintenance/manage_icgc.md) |
| Manage returned data | [wiki/maintenance/manage_dr](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/maintenance/manage_dr.md)
| Update the statistical plots | [wiki/maintenance/update_statistics](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/maintenance/update_statistics.md) |
| Update the Whole Genome analyses (expression, CNA and Data Integration) | [wiki/maintenance/update_wg_plots](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/maintenance/update_wg_plots.md)

### Active development

**Please note:** Despite PED analytics is a resource designed to be modular and easy-to-implement, a mid-level experience in web development is essential. In particular, the front-end requires knowledge in PHP and JavaScript; all backend functions were developed by using python (version 3.6) and R.

| Description | Details |
| ------------|---------|
| Implement a new analysis | [wiki/active_dev/implement_analysis](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/active_dev/implement_analysis.md) |
| Add a new data source | [wiki/active_dev/add_source](https://github.com/wynstep/PED_Analytics_UG/blob/master/wiki/active_dev/add_source.md) |

## Permissions for accessing to this documentation

This github repository is __private__ and the access is granted just to authorised researchers. In case of enquiries please contact the [mantainer](mailto:s.pirro@qmul.ac.uk) of the resource.
