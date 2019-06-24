## Manage PubMed articles

All the information related to PubMed articles (PMID, Title, Authors, PubDate, Abstract, Clinical/Molecular features, MeSH terms) are stored into a tab-separated file located in `/src` (`literature.tsv`).

For __adding a new article__ in the PubMed section, you just need to add a new row into this file using any technology you prefer (Microsoft Excel, R).
__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

[SMAC]() represents the fastest and easier way for retrieving new relevant articles from PubMed, together with associated -omics data and bioinformatics results.
Indeed, SMAC allows the user to provide a list of PMID to parse and analyse in a fast and easy way. If an article does not have any associated GEO dataset, it's just excluded from the analysis. Please refer to [this documentation]() for further details. This section will simulate a real case of use, by using a __SMAC-mediated procedure__ and a __manual procedure__.

_Add an article with PMID_ __()__ _and associated GEO dataset_ __()__ (expression data from array)

* [SMAC-mediated procedure](#SMAC-mediated-procedure)
* [Manual procedure](#manual-procedure)


Please click [here](#additional-files) if you need details about the file format for Copy Number Alterations, Mutations and Gene Fusion.

### SMAC-mediated procedure

* Create a text file that we'll name `pmid_new.txt` and write __PMID__ inside;
* Launch SMAC by using the command `ciao` and wait for the procedure to be completed.
*

### Manual procedure

* Open the file `/src/literature.tsv` using R (preferred) or Microsoft Excel and update with the details of the PubMed article, according to the aforementioned section
* Create a folder named `PMID` located in `/data`
* Create a subfolder named `GSE` located in `/data/PMID`
* Create the file _target.1.tsv_, a tab-separated file that contains a description of the type of samples taken into account for the analysis. The format of the file is reported here:

| Sample name | Target | Feature1 | Feature2 | .... |
| --------- | ------ | ------ | ------ | ---- |
| GSM... | | | |
| GSM... | | | |
| GSM... | | | |
| ... | | | |

* Create the file _eData.1.tsv_, a tab-separated file that contains the expression levels for the genes included in the analysis. __Sample names in the target and eData files need to be the same__, otherwise the analysis will not work properly. A template of the file is reported below:

| Gene name | GSM... | GSM... | GMS... | .... |
| --------- | ------ | ------ | ------ | ---- |
| Gene1 | | | |
| Gene2 | | | |
| Gene3 | | | |
| ... | | | |

* Perform the bioinformatics analyses on the datasets

| Analysis | Command |
| ------ | ---- |
| PCA | |
| Tumour purity | |
| Gene expression (WG) | |

* Update the statistics about number of samples and data
``

### Additional files

__Copy Number Alteration__: _cna.1.tsv_
| Gene name | GSM... | GSM... | GMS... | .... |
| --------- | ------ | ------ | ------ | ---- |
| Gene1 | | | |
| Gene2 | | | |
| Gene3 | | | |
| ... | | | |

__Mutations__: _mutations.1.tsv_
| Gene name | GSM... | GSM... | GMS... | .... |
| --------- | ------ | ------ | ------ | ---- |
| Gene1 | | | |
| Gene2 | | | |
| Gene3 | | | |
| ... | | | |

_mutations.chr.1.tsv_
| Gene name | GSM... | GSM... | GMS... | .... |
| --------- | ------ | ------ | ------ | ---- |
| Gene1 | | | |
| Gene2 | | | |
| Gene3 | | | |
| ... | | | |

__Gene Fusion__: _fusion.1.tsv_
| Gene name | GSM... | GSM... | GMS... | .... |
| --------- | ------ | ------ | ------ | ---- |
| Gene1 | | | |
| Gene2 | | | |
| Gene3 | | | |
| ... | | | |
