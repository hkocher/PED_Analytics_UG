## Manage PubMed articles

All the information related to PubMed articles (PMID, Title, Authors, PubDate, Abstract, Clinical/Molecular features, MeSH terms) is stored into a tab-separated file located in `/src` (`literature.tsv`).

For __adding a new article__ in the PubMed section, you just need to add a new row into this file using any technology you prefer (Microsoft Excel, R).
__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

[SMAC](https://hub.docker.com/r/hfx320/smac) represents the fastest and easier way for retrieving new relevant articles from PubMed, together with associated -omics data and bioinformatics results.
Indeed, SMAC allows the user to provide a list of PMID to parse and analyse in a fast and easy way. If an article does not have any associated GEO dataset, it's just excluded from the analysis. Please refer to [this documentation](https://github.com/wynstep/SMAC) for further details.
This section will simulate a real case of use, by using a __SMAC-mediated procedure__ and a __manual procedure__.

> _Add an article with PMID_ __16053509__ _and associated GEO dataset_ __GSE1542__ (expression data from array)

+ [SMAC-mediated procedure](#SMAC-mediated-procedure)
	1. [Run SMAC](#run-SMAC)
	2. [Copy results from SMAC to PED server](#copy-results-from-SMAC-to-PED-server)
	3. [create gene list](#create-gene-list)
	4. [create whole genome analysis](#create-whole-genome-analysis)
	5. [Update statistics](#update-statistics)
+ [Manual procedure](#manual-procedure)
	1. [Map PMID information](#map-PMID-information)
	2. [Store omics data](#store-omics-data)
	3. [Perform bioinformatics analysis](#perform-bioinformatics-analysis)
	4. [Create gene list](#create-gene-list)
	5. [Update statistics](#update-statistics)

### SMAC-mediated procedure

##### Run SMAC
* Create a text file that we'll name `pmid_new.txt` and write __16053509__ inside;
* Launch SMAC by using the command
> `docker run -v ./:/smac/results hfx320/smac:latest -p ./pmid_new.txt -s 0 -e <your email address>` and wait for the procedure to be completed.
* Once the analysis is completed, a folder with an _hex random code_ is created in the output path.
##### Copy results from SMAC to PED server
* Copy the folder `<hex_code>/data/geo_datasets/16053509` to the PED server folder `/data/PMID`
* Map the information stored into the file `<hex_code>/data/literature.tsv` to the PED server file `/src/literature.tsv`
##### Create gene list
* On the PED server, run the script
> `python scripts/data_preparation/create_gene_list.py --dir data/16053509/GSE1542`
##### Create Whole Genome analysis
In this example, the selected dataset has just expression data associated. We developed a script that is able to detect the presence of expression, CNA and mutations, for generating whole-genome CIRCOS plots, accordingly.
> `python scripts/data_preparation/create_circos.py --dir data/16053509/GSE1542`
##### Update statistics
I created a python script that aims at updating the statistics of PED analytics by generating an interactive plot. The script is named __calculate_statistics.py__ and is located in `scripts/data_preparation`. In order to generate the updated statistics, we need to run:

> python scripts/data_preparation/calculate_statistics.py --folder data/ --outdir src/stats/

### Manual procedure

##### Map PMID information
* Open the file `/src/literature.tsv` using R (preferred) or Microsoft Excel and update with the details of the PubMed article, according to the aforementioned section
##### Store omics data
* Create a folder named `PMID` located in `/data`
* Create a subfolder named `GSE` located in `/data/PMID`
* Create the file _target.1.tsv_, a tab-separated file that contains a description of the type of samples taken into account for the analysis. The format of the file is reported here:

| File_name | Target |
| ------ | ---- |
| GSM26248 | Normal |
| GSM26252 | Normal |
| GSM26295 | PDAC |
| GSM26296 | Normal |
| ... | ... |

* Create the file _eData.1.tsv_, a tab-separated file that contains the expression levels for the genes included in the analysis. __Sample names in the target and eData files need to be the same__, otherwise the analysis will not work properly. A template of the file is reported below:

| Gene name | GSM26248 | GSM26252 | GSM26295 | GSM26296 | ... |
| --------- | ------ | ------ | ------ | ---- | --- |
| DDR1 | 156.9 | 105.3 | 223.8 | 173.5 | ... |
| HSPA6 | 55.3 | 28.3 | 76.6 | 34.4 | ... |
| PAX8 | 191.3 | 294.2 | 324.6 | 531.9 | ... |
| ... | ... | ... | ... | ... |

Please click [here](#additional-files) if you need details about the file format for Copy Number Alterations, Mutations and Gene Fusion.

##### Perform bioinformatics analysis

| Analysis | Command |
| ------ | ---- |
| PCA | `Rscript scripts/R_scripts/PCA.R -e data/16053509/GSE1542/eData.1.tsv -t data/16053509/GSE1542/target.1.tsv -c target -u array -p 3 -d data/16053509/GSE1542/` |
| Tumour purity | `Rscript scripts/R_scripts/tumour_purity.R -e data/16053509/GSE1542/eData.1.tsv -t data/16053509/GSE1542/target.1.tsv -c target -d data/16053509/GSE1542/` |
| Whole genome plots | `python scripts/data_preparation/create_circos.py --dir data/16053509/GSE1542` |

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
| --------- | ------ | ------ | ------ | ---- | --------- | ------ | ------ |
| Sample1 | EGFR | EGFR fusion | VICC | yes | unknown | unknown | ... |
| Sample2 | MAP2K4 | MAP2K4-intragenic | VICC | yes | unknown | unknown | ... |
| Sample3 | SMAD4 | SMAD4-intragenic | VICC | yes | unknown | unknown | ... |
| ... | ... | ... | ... | ... | ... | ... | ... |
