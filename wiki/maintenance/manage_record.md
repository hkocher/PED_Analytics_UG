## Manage a new record in TCGA, CCLE, Genie articles

All the information related to TCGA, CCLE and Genie is stored into a tab-separated file located in `/src` (`tcga.tsv, ccle.tsv, genie.tsv`).

Here we briefly report the full strategy for adding a new record in TCGA. The protocol would be the same for the other sources listed here.
1. [Add a new sample in the source file](#add-a-new-sample-in-the-source-file)
2. [Update molecular data](#update-molecular-data)
3. [Update gene list](#update-gene-list)
4. [Update bioinformatics analyses](#update-bioinformatics-analyses)
5. [Update statistics](#update-statistics)

### Add a new sample in the source file

For __adding a new sample__ in one of these datasources, you just need to add a new row into the corrensponding file using any technology you prefer (Microsoft Excel, R).

__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

### Update molecular data

* Go to the folder located in `data/tcga/pancreatic_cancer`
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

### Update gene list
I created a python script dedicated to the update of the gene lists, starting from selected molecular data. The script is named __create_gene_list.py__ and is located in `scripts/data_preparation`. In order to generate the updated gene list in our example, we need to run:

> python scripts/data_preparation/create_gene_list.py --folder data/tcga/pancreatic_cancer

### Perform bioinformatics analysis

| Analysis | Command |
| ------ | ---- |
| PCA | `Rscript scripts/R_scripts/PCA.R -e data/tcga/pancreatic_cancer/eData.1.tsv -t data/tcga/pancreatic_cancer/target.1.tsv -c target -u array -p 3 -d data/tcga/pancreatic_cancer/` |
| Tumour purity | `Rscript scripts/R_scripts/tumour_purity.R -e data/tcga/pancreatic_cancer/eData.1.tsv -t data/tcga/pancreatic_cancer/target.1.tsv -c target -d data/tcga/pancreatic_cancer/` |
| Gene expression (WG) | `python scripts/data_preparation/create_circos.py --dir data/tcga/pancreatic_cancer` |

### Update statistics
I created a python script that aims at updating the statistics of PED analytics by generating an interactive plot. The script is named __calculate_statistics.py__ and is located in `scripts/data_preparation`. In order to generate the updated statistics, we need to run:

> python scripts/data_preparation/calculate_statistics.py --folder data/ --outdir src/stats/


#### Additional files

__Copy Number Alteration__: _cna.1.tsv_

| Gene name | Sample1 | Sample2 | Sample3 | .... |
| --------- | ------ | ------ | ------ | ---- |
| Gene1 | 0.007 | 0.051 | 0.469 | ... |
| Gene2 | -0.351 | 0.000 | 0.160 | ... |
| Gene3 | -0.484 | -0.379 | -0.270 | ... |
| ... | ... | ... | ... | ... |

_cna.chr.1.tsv_

| Gene name | Chromosome | Position | Sample1 | Sample2 | Sample3 | .... |
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
