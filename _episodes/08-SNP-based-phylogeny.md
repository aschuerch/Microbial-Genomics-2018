---
title: "SNP based phylogenies"
teaching: 10
exercises: 90
questions:
- "How can a phylogenetic tree be produced?"
objectives:
- "Map reads against a reference genome"
- "Extract single nucleotide polymorphisms"
- "Establish a phylogenetic tree from the SNP data"
keypoints:
- "Single nucleotide polymorphisms can be identified by mapping reads to a reference genome"
- "Parameters for the analysis have to be selected based on expected outcomes for this organism"
- "Concatenation of SNPs helps to reduce analysis volume"    
- "Phylogenetic trees can be annotated with a bracket syntax in Newick format"
---

This episode describes the processes to create a phylogenetic tree from raw reads by comparing these against a reference genome.

Reference sequences (including many pathogen genomes) are available at [NCBI's refseq database](https://www.ncbi.nlm.nih.gov/refseq/)
Programmatically down-loadable with ftp (file transfer protocol)
wc [filename] gives information on size
grep pattern [filename] selects lines in files that match patt

## Selection of a reference genome

A reference genome is a genome that was previously sequenced and is closely related

### Download reference genomes from NCBI

Download the M.tuberculosis reference genome with:

~~~
ftp ftp.ncbi.nlm.nih.gov
~~~
{: .source}


After the prompt for user (`anonymous`) and password (your email address), type

~~~
cd /genomes/refseq/bacteria/Mycobacterium_tuberculosis/reference/GCF_000195955.2_ASM19595v2
get GCF_000195955.2_ASM19595v2_genomic.fna.gz
~~~
{: .source}


To close the program, type
~~~
bye
~~~
{: .source}


Extract the file by typing

~~~
gunzip GCF_000195955.2_ASM19595v2_genomic.fna.gz
~~~
{: .source}


> ## Challenge: How big is the genome?
>
> Find out how many basepairs the genome has. Hints:
> ~~~
> wc [filename]
> ~~~
> gives information on size.
> ~~~
> grep pattern [filename]
> ~~~
> selects lines in files that match patterns
>

> > ## Solution
> >
> > 
> > ~~~
> > [1] "grep -v '>' GCF_000195955.2_ASM19595v2_genomic.fna| wc -m"
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}




{% include links.md %}
