## Gene Expression

This analytical module visualises the distribution of mRNA expression measurements across all samples for a user-defined gene, as well as all the aberrantly expressed genes in the whole-genome.

* [Expression of a single gene, in all the samples of the subset](#gene-specific-analysis)
* [Expression of all the genes, in a selected sample](#sample-specific-analysis)

### Gene-specific analysis
By using the gene-specific feature you can view how the expression of your gene of interest varies across different biological conditions. To provide a comprehensive overview of expression values across the biological groups, results are presented as both summarised and a sample views (box plot and bar plot, respectively). The box plot demonstrates mRNA expression level quartiles (y-axis) in samples stratified according to their group (x-axis). It can be used to can view if, and how, the expression of your gene of interest varies across the different biological groups. To facilitate the understanding of any differences, a barplot in which the expression values of individual samples (bars) is also provided. It illustrates the expression level of the gene of interest in individual samples clustered by biological group and ordered within each group by expression value (y-axis) of the queried gene.

![gene_expr_1](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/gene_expr_1.png)
![gene_expr_2](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/gene_expr_2.png)

### Sample-specific analysis

The whole-genome results are visualised in a form of an interactive CIRCOS plot, where each track represents individual genetic events. The over-expressed (z-score > 1) and under-expressed (z-score < -1) genes are indicated as dark red and dark blue dots on the red and blue circles, respecively. Users can click on a chromosome to see a detailed view of the chromosome on the UCSC Genome Browser (bottom). Similarly, users can click on any part of a track, which will expand the respective chromosomal region in the UCSC genome browser.

![gene_expr_wg](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/gene_expr_wg.svg)
