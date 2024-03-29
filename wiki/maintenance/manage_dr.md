## Manage returned data

One of the key aspects of PED analytics is the inclusion of data generated by the analysis of samples released by the **Pancreatic Cancer Research Fund Tissue Bank (PCRFTB)** to external researchers. The number of returned data is expected to grow during time, so a correct maintenance of this section is fundamental.

__Please note:__ Most of the data are returned to the user in a _tabular format_, for this reason I designed an interactive framework (called __exploration__) that allow to interactively visualise the data.

> **Case of use:** Adding a new dataset returned with code **DR0004**

1. [Update source list](#update-source-list)
2. [Store omics data](#store-omics-data)
3. [Create a password for accessing the dataset](#create-a-password-for-accessing-the-dataset)
4. [Create gene list](#create-gene-list)
5. [Run bioinformatics analyses](#run-bioinformatics-analyses)
6. [Update statistics](#update-statistics)

### Update source list
For __adding a new dataset__ to the Returned Data group, you just need to add a **new row** into the file `src/dr.tsv` using any technology you prefer (Microsoft Excel, R). The structure of the file is the following:

| ID | BPTB_project | Title | Date | Analysis |
| -- | ------------ | ----- | ---- | -------- |
| DR0001 | 2016/03/QM/TC/E/Urine&tissue, <br> 2017/11/QM/TC/C/Plasma&Urine | Urine biomarkers from PDAC patients | 2017/11/01 | exploration |
| DR0002 | 2015/07/QM/CH/P/CTCBlood | Blood biomarkes from PDAC patients | 2015/07/01 | exploration |
| __DR0004__ | __Internal code__ | __Description of the project__ | __date__ | __exploration__ |

The last column refers to the bioinformatics analyses associated to the selected dataset. In the example, since the returned data consist of a table with such clinical values, we'll associate just the _exploration_ analysis. [Here](https://github.com/wynstep/PED_Analytics_UG#available-analyses) you can find a detailed list of all the available analyses.

__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

### Store omics data
* Create a folder named `DR0004` located in `/data/`
* Create the file _target.1.tsv_, a tab-separated file that contains a description of the type of samples taken into account for the analysis. The format of the file is reported here:

| File_name | Target |
| ------ | ---- |
| Sample1 | Normal |
| Sample2 | Normal |
| Sample3 | PDAC |
| Sample4 | Normal |
| ... | ... |

**IN CASE DATA RETURNED ARE IN FORM OF A TABLE**
* Create the file _exploration.1.tsv_, a tab-separated file that contains the experimental features monitored in the analysis. __Sample names in the target and exploration files need to be the same__, otherwise the analysis will not work properly. A template of the file is reported below:

| SampleID | Target | Feature1 | Feature2 | Feature3 | ... |
| --------- | ------ | ------ | ------ | ---- | --- |
| Sample1 | Normal | 105.3 | 223.8 | 173.5 | ... |
| Sample2 | Normal | 28.3 | 76.6 | 34.4 | ... |
| Sample3 | PDAC | 294.2 | 324.6 | 531.9 | ... |
| ... | ... | ... | ... | ... |

Please click [here](#additional-files) if you need details about the file format for Expression, Copy Number Alterations, Mutations and Gene Fusion.

### Create a password for accessing the dataset
The access to the samples returned by researchers is **password-protected** prior to the publication. The PCRFTB data access committee (DAC) is responsible for granting the access to the data to selected users. In order to guarantee a top-level security, passwords in PED analytics are encoded using md5.

* Open password file `src/pswd.txt` and add a **new row** into the file. The structure of the file is the following:

| StudyID | pswd | clear |
| -- | ------------ | ----- |
| DR0004 | md5 password | clear password |

Strong passwords can be created [here](https://passwordsgenerator.net)
MD5 encryption of the generated password can be done [here](https://www.md5hashgenerator.com)

### Create gene list
I created a python script dedicated to the update of the gene lists, starting from selected molecular data. The script is named __create_gene_list.py__ and is located in `scripts/data_preparation`. In order to generate the updated gene list in our example, we need to run:

> python scripts/data_preparation/create_gene_list.py --folder data/DR0004/

### Run bioinformatics analysis

__PLEASE NOTE__: these analysis can be run JUST if _expression data_ are available in your dataset, otherwise please skip this section.
| Analysis | Command |
| ------ | ---- |
| PCA | `Rscript scripts/R_scripts/PCA.R -e data/DR0004/eData.1.tsv -t data/DR0004/target.1.tsv -c target -u array -p 3 -d data/DR0004/` |
| Tumour purity | `Rscript scripts/R_scripts/PCA.R -e data/DR0004/eData.1.tsv -t data/DR0004/target.1.tsv -c target -d data/DR0004/` |
| Gene expression (WG) | `python scripts/data_preparation/create_circos.py --dir data/DR0004` |

### Update statistics
I created a python script that aims at updating the statistics of PED analytics by generating an interactive plot. The script is named __calculate_statistics.py__ and is located in `scripts/data_preparation`. In order to generate the updated statistics, we need to run:

> python scripts/data_preparation/calculate_statistics.py --folder data/ --outdir src/stats/

### Additional files

__Expression__: _eData.1.tsv_

| Gene name | Sample1 | Sample2 | Sample3 | Sample4 | ... |
| --------- | ------ | ------ | ------ | ---- | --- |
| DDR1 | 156.9 | 105.3 | 223.8 | 173.5 | ... |
| HSPA6 | 55.3 | 28.3 | 76.6 | 34.4 | ... |
| PAX8 | 191.3 | 294.2 | 324.6 | 531.9 | ... |
| ... | ... | ... | ... | ... |

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
| --------- | ------ | ------ | ------ | ---- | --------- | ------ | ------ |
| Sample1 | EGFR | EGFR fusion | VICC | yes | unknown | unknown | ... |
| Sample2 | MAP2K4 | MAP2K4-intragenic | VICC | yes | unknown | unknown | ... |
| Sample3 | SMAD4 | SMAD4-intragenic | VICC | yes | unknown | unknown | ... |
| ... | ... | ... | ... | ... | ... | ... | ... |
