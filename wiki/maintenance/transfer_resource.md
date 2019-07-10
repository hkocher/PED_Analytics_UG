## Transfer the resource

PED analytics has been designed to be easy to maintain and manage. It does not rely on any SQL database and all the required files for showing the interface are included in a single folder. PED analytics includes two main branches/versions:

Version | Description | directory
------------ | ------------ | ------------
Live | Stable and bug-free | /var/www/html/bioinf |
Beta version | Environment for testing new features and adding datasets | /var/www/html/bioinf/beta_version |

In order to transfer the resource, please just copy and paste the live/beta folder in the new location.

Obviously, most functionalities of PED will not work out-of-box in the new server, some R/python packages have to be installed and also several variables have to be updated.

### Dependencies and packages

Package | Environment | Version | Required by | Command to install |
------- | ------------| ------- | ----------- | ----- |
plotly | R | 4.9.0 | all R-based analyses | `install.packages('plotly')` |
optparse | R | 1.6.2 | all R-based analyses | `install.packages('optparse')` |
data.table | R | 1.12.2 | all R-based analyses | `install.packages('data.table')` |
heatmaply | R | 0.16.0 | correlation.R, FrequencyPerGeneChr.R | `install.packages('heatmaply')` |
psych | R | 1.8.12 | correlation.R | `install.packages('psych')` |
dplyr | R | 0.8.3 | fusion.R, mutation_frequency.R | `install.packages('dplyr')` |
visNetwork | R | 2.0.7 | gene_network.R | `install.packages('visNetwork')` |
TCGAbiolinks | R | 2.12.2 | mutations.R | `BiocManager::install("TCGAbiolinks")` |
pdftools | R | 2.2 | mutations.R | `install.packages('pdftools')` |
R2HTML | R | 2.3.2 | mutations.R, survival.R | `install.packages('R2HTML')` |
DT | R | 0.7 | mut_batch_search.R | `install.packages('DT')` |
tidyr | R | 0.8.3 | mut_batch_search.R | `install.packages('tidyr')` |
R2HTML | R | 2.3.2 | mutations.R, survival.R | `install.packages('R2HTML')` |
estimate | R | 1.0.13 | tumour_purity.R | `library(utils); rforge <- "http://r-forge.r-project.org"; install.packages("estimate", repos=rforge, dependencies=TRUE)` |
biomaRt | R | 2.40.1 | create_circos_file.R | `BiocManager::install("biomaRt")` |
plotly | python | 3.10.0 | calculate_statistics.py | `pip install plotly` |
pathlib | python | 1.0.1 | all python scripts | `pip install pathlib` |
argparse | python | 1.4.0 | all python scripts | `pip install argparse` |
pandas | python | 0.24.2 | icgc_list_to_matrix.py | `pip install pandas` |
numpy | python | 1.16.4 | icgc_list_to_matrix.py | `pip install numpy` |
mygene | python | 3.1.0 | icgc_list_to_matrix.py | `pip install mygene` |

### Variables to be updated
All the variables that maintain linkage consistency between files, are stored in:
* `script/vars.php`
* `scripts/R_scripts/vars.R`
* `web_vars.php`

Please refer to the comments inside each file, for a detailed description of each variable.
