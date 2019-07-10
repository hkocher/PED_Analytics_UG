## Manage ICGC records

ICGC is an international effort for collecting, organising and integrating molecula and clinical information for a wide range of cancers.
PED analytics includes all the pancreas-related datasets currently released in ICGC. However, new datasets may be released in the future.

Here we cover two possible cases of use:
+ [Add a new sample to existing dataset](#add-a-new-sample-to-existing-dataset)
	1. [Update sample list](#update-sample-list)
	2. [Update molecular data](#update-molecular-data)
	3. [Update gene list](#update-gene-list)
	4. [Run bioinformatics analyses](#run-bioinformatics-analyses)
	5. [Update statistics](#update-statistics)
+ [Add a new dataset](#add-a-new-dataset)
	1. [Update source list](#update-source-list)
	2. [Store omics data](#store-omics-data)
	3. [Create gene list](#create-gene-list)
	4. [Run bioinformatics analyses](#run-bioinformatics-analyses)
	5. [Update statistics](#update-statistics)

### Add a new sample to existing dataset
> **Case of use:** Adding a new sample to the **PAAD-US** dataset

##### Update sample list

For __adding a new sample__ in one of these datasources, you just need to add a **new row** into the file `data/icgc/PAAD-US/target.details.tsv` using any technology you prefer (Microsoft Excel, R).

__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

##### Update molecular data

* Go to the folder located in `data/icgc/PAAD-US`
* Update the file _target.1.tsv_ (a tab-separated file that contains a description of the type of samples taken into account for the analysis), by adding a **new row** with the new sample details. The format of the file is reported here:

| File_name | Target |
| ------ | ---- |
| Sample1 | Normal |
| Sample2 | Normal |
| Sample3 | PDAC |
| Sample4 | Normal |
| ... | ... |

* Update the file _eData.1.tsv_ (a tab-separated file that contains the expression levels for the genes included in the analysis), by adding a **new column**. __Sample names in the target and eData files need to be the same__, otherwise the analysis will not work properly. A template of the file is reported below:

| Gene name | Sample1 | Sample2 | Sample3 | Sample4 | ... |
| --------- | ------ | ------ | ------ | ---- | --- |
| DDR1 | 156.9 | 105.3 | 223.8 | 173.5 | ... |
| HSPA6 | 55.3 | 28.3 | 76.6 | 34.4 | ... |
| PAX8 | 191.3 | 294.2 | 324.6 | 531.9 | ... |
| ... | ... | ... | ... | ... |

Please click [here](#additional-files) if you need details about the file format for Copy Number Alterations, Mutations and Gene Fusion.

##### Update gene list
I created a python script dedicated to the update of the gene lists, starting from selected molecular data. The script is named __create_gene_list.py__ and is located in `scripts/data_preparation`. In order to generate the updated gene list in our example, we need to run:

> python scripts/data_preparation/create_gene_list.py --folder data/icgc/PAAD-US

##### Run bioinformatics analysis

| Analysis | Command |
| ------ | ---- |
| PCA | `Rscript scripts/R_scripts/PCA.R -e data/icgc/PAAD-US/eData.1.tsv -t data/icgc/PAAD-US/target.1.tsv -c target -u array -p 3 -d data/icgc/PAAD-US/` |
| Tumour purity | `Rscript scripts/R_scripts/tumour_purity.R -e data/icgc/PAAD-US/eData.1.tsv -t data/icgc/PAAD-US/target.1.tsv -c target -d data/icgc/PAAD-US/` |
| Gene expression (WG) | `python scripts/data_preparation/create_circos.py --dir data/icgc/PAAD-US` |

##### Update statistics
I created a python script that aims at updating the statistics of PED analytics by generating an interactive plot. The script is named __calculate_statistics.py__ and is located in `scripts/data_preparation`. In order to generate the updated statistics, we need to run:

> python scripts/data_preparation/calculate_statistics.py --folder data/ --outdir src/stats/

### Add a new dataset
> **Case of use:** Adding a new dataset named **PAAD-CN** which contains PDAC samples from chinese population

##### Update source list
For __adding a new dataset__ in one ICGC, you just need to add a **new row** into the file `src/icgc.tsv` using any technology you prefer (Microsoft Excel, R).

__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

##### Store omics data
* Create a folder named `PAAD-CN` located in `/data/icgc`
* Create the file _target.1.tsv_, a tab-separated file that contains a description of the type of samples taken into account for the analysis. The format of the file is reported here:

| File_name | Target |
| ------ | ---- |
| Sample1 | Normal |
| Sample2 | Normal |
| Sample3 | PDAC |
| Sample4 | Normal |
| ... | ... |

* Create the file _eData.1.tsv_, a tab-separated file that contains the expression levels for the genes included in the analysis. __Sample names in the target and eData files need to be the same__, otherwise the analysis will not work properly. A template of the file is reported below:

| Gene name | Sample1 | Sample2 | Sample3 | Sample4 | ... |
| --------- | ------ | ------ | ------ | ---- | --- |
| DDR1 | 156.9 | 105.3 | 223.8 | 173.5 | ... |
| HSPA6 | 55.3 | 28.3 | 76.6 | 34.4 | ... |
| PAX8 | 191.3 | 294.2 | 324.6 | 531.9 | ... |
| ... | ... | ... | ... | ... |

Please click [here](#additional-files) if you need details about the file format for Copy Number Alterations, Mutations and Gene Fusion.

##### Run bioinformatics analysis

| Analysis | Command |
| ------ | ---- |
| PCA | `Rscript scripts/R_scripts/PCA.R -e data/icgc/PAAD-CN/eData.1.tsv -t data/icgc/PAAD-CN/target.1.tsv -c target -u array -p 3 -d data/icgc/PAAD-CN/` |
| Tumour purity | `Rscript scripts/R_scripts/PCA.R -e data/icgc/PAAD-CN/eData.1.tsv -t data/icgc/PAAD-CN/target.1.tsv -c target -d data/icgc/PAAD-CN/` |
| Gene expression (WG) | `python scripts/data_preparation/create_circos.py --dir data/icgc/PAAD-CN` |

**Please Note**: all the other bioinformatics analyses in PED analytics are run in *live* and relies on the presence **at least** of the expression data (eData) and the target file.

##### Update statistics
I created a python script that aims at updating the statistics of PED analytics by generating an interactive plot. The script is named __calculate_statistics.py__ and is located in `scripts/data_preparation`. In order to generate the updated statistics, we need to run:

> python scripts/data_preparation/calculate_statistics.py --folder data/ --outdir src/stats/

### Additional files

__Copy Number Alteration__: _cna.1.tsv_

| Gene name | GSM... | GSM... | GMS... | .... |
| --------- | ------ | ------ | ------ | ---- |
| Gene1 | 0.007 | 0.051 | 0.469 | ... |
| Gene2 | -0.351 | 0.000 | 0.160 | ... |
| Gene3 | -0.484 | -0.379 | -0.270 | ... |
| ... | ... | ... | ... | ... |

_cna.chr.1.tsv_

| Gene name | Chromosome | Position | GSM... | GSM... | GMS... | .... |
| --------- | ---------- | -------- | ------ | ------ | ------ | ---- |
| Gene1 | 1 | 1292376 | 0.007 | 0.051 | 0.469 | ... |
| Gene2 | 9 | 3021483 | -0.351 | 0.000 | 0.160 | ... |
| Gene3 | 5 | 1020123 | -0.484 | -0.379 | -0.270 | ... |
| ... | ... | ... | ... | ... | ... | ... |

__Mutations__: _mutations.1.tsv_

| sample | gene | effect | chr | start | end | reference | alt |
| ------ | ---- | ------ | --- | ----- | --- | --------- | --- |
| Sample 1 | PLEK | Frame Shift Deletion | chr2 | 68622834 | 68622834 | C | - |
| Sample 2 | C9orf173 | Frame Shift Insertion | chr9 | 140146549 | 140146550 | - | - |
| Sample 3 | EXPH5 | Frame Shift Insertion | chr11 | 108382677 | 108382678 | - | - |
| ... | ... | ... | ... | ... | ... | ... | ... |

__Gene Fusion__: _fusion.1.tsv_

| TUMOR_SAMPLE_BARCODE | HUGO_SYMBOL | FUSION | CENTER | DNA_SUPPORT | RNA_SUPPORT | FRAME | COMMENTS |
| --------- | ------ | ------ | ------ | ---- | --------- | ------ | ------ | ------ | ---- |
| Sample1 | EGFR | EGFR fusion | VICC | yes | unknown | unknown | |
| Sample2 | MAP2K4 | MAP2K4-intragenic | VICC | yes | unknown | unknown |  |
| Sample3 | SMAD4 | SMAD4-intragenic | VICC | yes | unknown | unknown |  |
| ... | ... | ... | ... | ... | ... | ... | ... |
