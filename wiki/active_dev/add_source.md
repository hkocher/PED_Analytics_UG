## Add a new source of data

The operation of adding a complete new source of data, is not frequent and is expected to be performed **on each version update** (every 18 months). In this section we'll try to simulate the creation and addition of a new, dummy datasource.

> **Case of use:** Adding a new source of data called **PanCa**. This datasource contains 3 sub-projects called **PanCa_IT, PanCa_US, PanCA_UK**

+ [Backend preparation](#backend-preparation)
	1. [Create source list](#create-source-list)
	2. [Store omics data](#store-omics-data)
	3. [Create gene list](#create-gene-list)
	4. [Run bioinformatics analyses](#run-bioinformatics-analyses)
	5. [Update statistics](#update-statistics)
+ [Frontend modifications](#frontend-modifications)
	1. [Create a dedicated page](create-a-dedicated-page)
	2. [Add a link to the datasource](add-a-link-to-the-datasource)

### Backend preparation

#### Create source list
The source list is a tab-delimited file that should itemise all the samples/sub-datasets that are part of the resource. In our case of use, the PanCa datasource contains 3 sub-projects that we'll list here.

* create the file `panca.tsv` located in `src` folder

| ID | Description | Num samples | ... | Analysis |
| -- | ------------ | ----- | ---- | -------- |
| PanCa_IT | PanCa samples from Italy | 150 | ... | details,pca,tumour_purity,expression,cna,mutations |
| PanCa_US | PanCa samples from USA | 2300 | ... | details,pca,tumour_purity,expression,mutations |
| PanCa_UK | PanCa samples from UK | 200 | ... | details,pca,tumour_purity,expression |

The last column refers to the bioinformatics analyses associated to the selected dataset. [Here](https://github.com/wynstep/PED_Analytics_UG#available-analyses) you can find a detailed list of all the available analyses.

__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

#### Store omics data
* Create a folder named `panca` located in `/data/`
* Create 3 subfolders named `PanCa_IT`, `PanCa_US` and `PanCa_UK`, located in `/data/panca/`

**For each subproject:**
* Create the file _target.1.tsv_, a tab-separated file that contains a description of the type of samples taken into account for the analysis. The format of the file is reported here:

| File_name | Target |
| ------ | ---- |
| Sample1 | Normal |
| Sample2 | Normal |
| Sample3 | PDAC |
| Sample4 | Normal |
| ... | ... |

* Create the file _target.details.tsv_, a tab-separated file that contains the full description of the samples in the dataset. This can be considered an extended version of _data.1.tsv_.

* Create the _eData.1.tsv_, _cna.1.tsv_, _mutations.1.tsv_, _fusion.1.tsv_, according to the type of data available for each dataset. Please click [here](#additional-files) if you need details about the file format for these data type.

#### Create gene list
I created a python script dedicated to the update of the gene lists, starting from selected molecular data. The script is named __create_gene_list.py__ and is located in `scripts/data_preparation`. In order to generate the updated gene list in our example, we need to run:

> python scripts/data_preparation/create_gene_list.py --folder data/panca/PanCa_IT<br>
> python scripts/data_preparation/create_gene_list.py --folder data/panca/PanCa_US<br>
> python scripts/data_preparation/create_gene_list.py --folder data/panca/PanCa_UK

#### Run bioinformatics analysis

**For each subproject:**

| Analysis | Command |
| ------ | ---- |
| PCA | `Rscript scripts/R_scripts/PCA.R -e data/panca/<panca_subproject>/eData.1.tsv -t data/panca/<panca_subproject>/target.1.tsv -c target -u array -p 3 -d data/panca/<panca_subproject>/` |
| Tumour purity | `Rscript scripts/R_scripts/tumour_purity.R -e data/panca/<panca_subproject>/eData.1.tsv -t data/panca/<panca_subproject>/target.1.tsv -c target -d data/panca/<panca_subproject>/` |
| Whole genome plots | `python scripts/data_preparation/create_circos.py --dir data/panca/<panca_subproject>` |

#### Update statistics
I created a python script that aims at updating the statistics of PED analytics by generating an interactive plot. The script is named __calculate_statistics.py__ and is located in `scripts/data_preparation`. In order to generate the updated statistics, we need to run:

> python scripts/data_preparation/calculate_statistics.py --folder data/ --outdir src/stats/

### Frontend modifications

#### Create a dedicated page
From the frontend side, we need to create a landing page for the user to the newly-created dataset. For doing this, the page needs to be located in `pages/sections/`. In our case of use, we'll create a page named `panca.php` in the mentioned directory.

As already stated in the user-guide, the active development requires a good knowledge of PHP and Javascript. Although the code is exhaustively commented, a strong programmatic background is necessary for understanding codes and procedures.

Anyway, in this particular example we are in front of a situation with **project/subproject** which a structure similar to the `icgc` dataset. For this reason, we can use the `pages/sections/icgc.php` page as template and modify it, accordingly.
> **Edits:** <br>
	`$id = "panca"`<br>
	`$type_analysis = "panca"`<br>
	`$base_ds = "panca"`

#### Add a link to the datasource
* Apply modification to the file `pages/sections/home.php`:
> `<a class="analysis_sel" id="literature" href="index.php?s=literature"> PubMed </a>`<br>
	`<a class="analysis_sel" id="tcga" href="index.php?s=tcga"> The Cancer Genome Atlas </a>`<br>
	`<a class="analysis_sel" id="ccle" href="index.php?s=ccle"> Cancer Cell Line Encyclopedia </a>`<br>
	`<a class="analysis_sel" id="genie" href="index.php?s=genie"> Genie </a>`<br>
	`<a class="analysis_sel" id="icgc" href="index.php?s=icgc"> ICGC </a>`<br>
	`<a class="analysis_sel" id="dr" href="index.php?s=data_returned"> Data returned </a>`<br>
	**`<a class="analysis_sel" id="panca" href="index.php?s=panca"> PanCA </a>`**

* Apply modification to the file `pages/web_vars.php`:
> `$selector = <<<HERE`<br>
	`<a class="analysis_sel" id="literature" href="index.php?s=literature"> PubMed </a>`<br>
	`<a class="analysis_sel" id="tcga" href="index.php?s=tcga"> The Cancer Genome Atlas </a>`<br>
	`<a class="analysis_sel" id="ccle" href="index.php?s=ccle"> Cancer Cell Line Encyclopedia </a>`<br>
	`<a class="analysis_sel" id="genie" href="index.php?s=genie"> Genie </a>`<br>
	`<a class="analysis_sel" id="icgc" href="index.php?s=icgc"> ICGC </a>`<br>
	`<a class="analysis_sel" id="dr" href="index.php?s=data_returned"> Data returned </a>`<br>
	**`<a class="analysis_sel" id="panca" href="index.php?s=panca"> PanCA </a>`**

#### Additional files

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
| --------- | ------ | ------ | ------ | ---- | --------- | ------ | ------ | ------ | ---- |
| Sample1 | EGFR | EGFR fusion | VICC | yes | unknown | unknown | |
| Sample2 | MAP2K4 | MAP2K4-intragenic | VICC | yes | unknown | unknown |  |
| Sample3 | SMAD4 | SMAD4-intragenic | VICC | yes | unknown | unknown |  |
| ... | ... | ... | ... | ... | ... | ... | ... |
