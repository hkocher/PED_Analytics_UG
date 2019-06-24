## Gene networks

For each comparative group defined in the dataset, the interactions between genes of interest and their primary neighbours can be displayed in an interactive network. In the resulting gene networks, nodes represent the genes while edges represent the interactions. Nodes are coloured according to the expression level (z-score) in the dataset of interest.

Color | Value
------------ | ------------
**RED** | z-score > 0
**BLUE** | z-score < 0
**GREY** | Expression data not available

![network](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/gene_network.png)

Additionally, an interactive table show the details of each source-target tuple by listing:
* a *score* (extracted from [Mentha](http://mentha.uniroma2.it)) positively-correlated with the strength of interaction
* a set of relevant PubMed articles that support the experimental validation of the *interaction*.

![network_table](https://github.com/wynstep/PED_Analytics_UG/blob/master/img/gene_network_table.png)
