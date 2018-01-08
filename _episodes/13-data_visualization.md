---
title: "Data Visualization"
teaching: 10
exercises: 30
questions:
- "How are phylogenetic trees viewed and compared?"
- "How can I visualize several layers of data?"
objectives:
- "Explore the resulting trees in conjunction with the meta data."
- "Make an estimation on the likelihood of transmission events"
keypoints:
- "Different genetic analysis methods can give different results"
- "Genetic information can confirm or contradict the meta data"
---

## Comparison of clustering results

We now grouped our isolates by two different methods based on the information that extracted from the sequencing reads.
In the first case, we compared our isolates to a reference genome and clustered them on basis of the single nucleotide variations that they exhibit when compared to this reference. In the second case we annotated genes in the assembled genomes and looked for genes that were variable present or absent.

[workflow](../fig/Workflow.png)


Now we will visualize and compare these two trees with each other. For that we will be using [Dendroscope](http://dendroscope.org/) which is a tree viewer that allows to make tanglegrams (showing and comparing two trees). Make sure you have both newick files (accessory_binary_genes.fa.newick and snptree.newick) on your own computer. 

On your own computer, open Dendroscope 3.

On the left, go to File > Open and choose snptree.newick.

Now, go to File > Add from File > and select accessory_binary_genes.fa.newick
 
Go to View > More panels (this has to be done while you have the first tree selected, you can see which one you have in the bottom left corner, navigate between them using the arrows in the top left corner). The second tree should now pop into view, alongside the first.

Go to Algorithms > Tanglegram. This will show you which samples have the same name.

> ## Exercise
>
> 1. Which grouping does the SNP tree suggest?
> 2. Which grouping does the pangenome tree suggest?
> 3. Where is the grouping consistent and where not?
> 4. Which grouping do you trust more and why?
{: .challenge}


## Visualization of genetic information and metadata

Visualization is frequently used to aid the interpretation of complex datasets. Within microbial genomics, visualizing the relationships between multiple genomes as a tree provides a framework onto which associated data (geographical, temporal, phenotypic and epidemiological) are added to generate hypotheses and to explore the dynamics of the system under investigation.

Download the meta data [file](../files/meta.csv) to your computer. Open Chrome. Go to [microreact](https://microreact.org/). Click on 'Upload'. Upload the newick tree produced by SNP typing **or** by the pangenome analysis and the metadata file.

Explore the location, time and further meta data.

> ## Exercise
>
> 1. Which grouping does the location information suggest?
> 2. Which grouping does the other metadata suggest?
> 2. Is this supported by the SNP clusters?
> 3. Where is the grouping consistent and where not?
> 4. Draw a transmission tree where possible
{: .challenge}


