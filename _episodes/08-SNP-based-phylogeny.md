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

This episode describes how to get all necessary data and the processes to create a phylogenetic tree from raw reads by comparing these against a reference genome.

Reference sequences (including many pathogen genomes) are available at [NCBI's refseq database](https://www.ncbi.nlm.nih.gov/refseq/)

## Selection of a reference genome

A reference genome is a genome that was previously sequenced and is closely related to the sequenced genome. The selection of a closely related reference genome is not trivial and can warrant an analysis in itself. However, for simplicity, here we will work with the *M. tuberculosis* reference genome H37Rv.

### Download reference genomes from NCBI

Download the *M.tuberculosis* reference genome from the NCBI ftp site.

First, we switch to the data folder to store all our data

~~~
cd dc_workshop/data 
~~~
{: .source}

With curl we use the -O flag, which simultaneously tells curl to download the page instead of showing it to us and specifies that it should save the file using the same name it had on the server:

~~~
curl -O ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/195/955/GCF_000195955.2_ASM19595v2/GCF_000195955.2_ASM19595v2_genomic.fna.gz
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
> > [1] grep -v '>' GCF_000195955.2_ASM19595v2_genomic.fna| wc -m
> > 4466677
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}



## Download the sequenced genomes from the European Nucleotide Archive (ENA)

To make room for our sequencing data we remove the remaining sample data from the machine.

~~~
rm -r data/
rm -r sra_metadata/
rm -r untrimmed_fastq/
rm -r .hidden
~~~
{: .source}


`wget` is short for “world wide web get”, and it’s basic function is to download web pages or data at a web address.
There are many repositories for public data. Some model organisms or fields have specific databases, and there are ones for particular types of data. Two of the most comprehensive are the National Center for Biotechnology Information (NCBI) and European Nucleotide Archive (EMBL-EBI). In this lesson we’re working with the ENA, but the general process is the same for any database.

Let's download our Mtb data with

~~~
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR029/ERR029207/*fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR029/ERR029206/*fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026478/*fastq.gz

wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026474/*fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026473/*fastq.gz

wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026481/*fastq.gz
wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026482/*fastq.gz
~~~
{: .source}

This will take a moment. 

Now, extract the reads with this loop

~~~
for reads in *fastq.gz
  do
    gunzip $reads
  done
~~~
{: .source}
    

{% include links.md %}
