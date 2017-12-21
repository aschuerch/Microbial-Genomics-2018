---
title: "Pangenome analysis"
teaching: 20
exercises: 60
questions:
- "How to determine a pangenome from a collection of isolate genome sequences?"
objectives:
- "Determine the best blast hit of genes."
- "Interpret the output of a pangenome analysis."
keypoints:
- "The microbial pangenome is the union of genes shared by genomes of interest."
- "Roary is a pipeline to determine genes of the pangenome."
---



## Pangenome analysis

The microbial pangenome is the union of genes shared by genomes of interest
Roary is a pipeline to determine genes of the pangenome
An excellent step-by-step tutorial can be found [here](https://github.com/microgenomics/tutorials/blob/master/pangenome.md)

### Extract genes recB, rubA and dnaA from two genomes.

For this, we need to find if the genes are present in the annotated files.

> ## Challenge: How can I extract genes from the file with the genes?
>
> Hints:
> ~~~
> grep
> ~~~
> > ## Solution
> >
> > 
> > ~~~
> > cd annnotation
> > [1] grep 'recA' *
> > 
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Note for each genome if the gene has been found or not. Mark them as present (1) in this [file](https://docs.google.com/spreadsheets/d/1xjiliy_USyMwiyzEgWhpn8_109F7Z3jPM_f7Jp-lOb8/edit?usp=sharing), else mark the gene that differs or is absent as 0.


Generate pangenome

Open [phandango](http://jameshadfield.github.io/phandango/) in Chrome, 

Drop file accessory_binary_genes.fa.newick and then file gene_absence_presence.csv
