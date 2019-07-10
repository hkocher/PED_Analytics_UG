## Implement a new bioinformatics analysis

PED analytics has been designed to be **modular** and **upgradable**. New functionalities can be developed and integrated into the resource in few steps. There are two types of analysis: **Pre-computed** and **Live**. The first are very easy to implement because they require just a landing page to show the results. The procedure for the second is little bit longer but not much complicated.

**The assumption for this section** is that the developer already created a fully functional R/python/whatever script for analysing the data. The requirements for the newly created script are:
* To take the arguments from the command line
* Generating output files named with a **custom name** and **custom location**

### Making the new analysis to be recognised by the system
The first, crucial step is allow the system to recognise the new analysis.
* Open the file `scripts/vars.php` and update the array `$an_whitelist` by adding the new analysis you implemented

+ [Pre-computed](#pre-computed)
	+ [Backend preparation](#backend-preparation)
		1. [Update source list](#create-source-list)
		2. [Run bioinformatics analyses](#run-bioinformatics-analyses)
	+ [Frontend modifications](#frontend-modifications)
		1. [Create a dedicated page](create-a-dedicated-page)
		2. [Link the system to landing page](link-to-the-landing-page)
+ [Live](#live)
	+ [Backend preparation](#backend-preparation)
		1. [Update source list](#create-source-list)
	+ [Frontend modifications](#frontend-modifications)
		1. [Create a landing page](create-a-landing-page)
		2. [Link the system to landing page](link-to-the-landing-page)
		2. [Make the analysis show the results](show-results)

### Pre-computed

### Backend preparation

#### Update source list
The source list is a tab-delimited file that should itemise all the samples/sub-datasets that are part of the resource. Source lists for all the datasources in PED analytics are locate in the `src` folder

The last column refers to the bioinformatics analyses associated to the selected dataset. [Here](https://github.com/wynstep/PED_Analytics_UG#available-analyses) you can find a detailed list of all the available analyses.
* Our aim is to **edit the last column** of the file for adding the new analysis, where necessary.

__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

#### Run bioinformatics analysis
Since this is a **pre-computed** bioinformatics analysis, we need to run the newly created script prior to the integration into the web interface. Just run your results locally and copy the output files in the server later, in your preferred location. By default, the pre-computed analysis are stored in `data/<data_set>/<sub-dataset>/`

### Frontend preparation

#### Create a dedicated page
Now that all the output files for the pre-computed analysis have been generated, we can create a landing page for showing the results.
* The page needs to be located in `pages/avail_analysis/`.

As already stated in the user-guide, the active development requires a good knowledge of PHP and Javascript. Although the code is exhaustively commented, a strong programmatic background is necessary for understanding codes and procedures.

For pre-computed analysis, I would suggest to use the page `pages/avail_analysis/pca.php` as template and modify it according to our needs.

#### Link to the landing page
The user can land to the page specific for the new results of analysis, by editing the `scripts/functions.php` page:
> **Edits:** <br>
	`function LoadPageAnalysis($valid_analysis, $id, $base_ds, $full_dir, $url_dir, $status) {`<br>
	`case "exploration":`<br>
	`include("$pages_dirname/avail_analysis/exploration.php"); break;`<br>
	**`case "new_analysis":`<br>
	`include("$pages_dirname/avail_analysis/new_analysis.php"); break;`**

### Live

### Backend preparation

#### Update source list
The source list is a tab-delimited file that should itemise all the samples/sub-datasets that are part of the resource. Source lists for all the datasources in PED analytics are locate in the `src` folder

The last column refers to the bioinformatics analyses associated to the selected dataset. [Here](https://github.com/wynstep/PED_Analytics_UG#available-analyses) you can find a detailed list of all the available analyses.
* Our aim is to **edit the last column** of the file for adding the new analysis, where necessary.

__Please note:__ Microsoft Excel automatically quotes all text-based fields in the file. Please remove the quotes, otherwise the file will not be correctly parsed by PED. I personally suggest to use R for editing the file. It's easier and faster.

### Frontend preparation

#### Create a landing page
We can create a landing page for submitting the analysis and locate in `pages/avail_analysis/`.

As already stated in the user-guide, the active development requires a good knowledge of PHP and Javascript. Although the code is exhaustively commented, a strong programmatic background is necessary for understanding codes and procedures.

For live analysis, I would suggest to use the page `pages/avail_analysis/gene_expression.php` as template and modify it according to our needs.

#### Link to the landing page
The user can land to the page specific for the new results of analysis, by editing the `scripts/functions.php` page:
> **Edits:** <br>
	`function LoadPageAnalysis($valid_analysis, $id, $base_ds, $full_dir, $url_dir, $status) {`<br>
	`case "exploration":`<br>
	`include("$pages_dirname/avail_analysis/exploration.php"); break;`<br>
	**`case "new_analysis":`<br>
	`include("$pages_dirname/avail_analysis/new_analysis.php"); break;`**

#### Show results
In order to make the live analysis showing the results, we need to edit the code in `scripts/run_analysis.php`. Just small editing needs to be done, here we list the methods to adjust, accordingly to the needs:

> **Edits:** <br>
	`switch($type_analysis){`<br>
	`case "exploration":`<br>
	`$command = "Rscript R_scripts/gene_expression.R -e ".$expression_file." -t ".$target_file." -c 'target' -p ".$genes." -d ".$tmp_dirname." -x ".$hexcode.""; break;`<br>
	**`case "new_analysis":`<br>
	`$command = "Rscript R_scripts/<new_analysis>.R -e ".$parameter1." -t ".$parameter2." -c 'target' -p ".$parameter3." -d ".$tmp_dirname." -x ".$hexcode.""; break;`**
