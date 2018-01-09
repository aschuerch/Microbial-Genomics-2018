---
title: "SNP phylogeny"
teaching: 15
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

In this episode we will try to pinpoint single nucleotide variants or single nucleotide polymorphism (SNPs) between our samples and the reference. The SNPs are determined by a process called read mapping in which they are aligned to the reference sequence.

![Mapping](../fig/snps_reads02.png)


The identified SNPs will be used to compare the isolates to each other and to establish a phylogenetic tree.

## Starting a new screen session

Again we will use a ‘screen session’ to do our calculation in the background. You can start a session and give it a descriptive name:

~~~
screen -S snp
~~~
{: .bash}

This creates a session with the name ‘snp’

As you work, this session will stay active until you close this session. Even if you disconnect from your machine, the jobs you start in this session will run till completion.

## SNP calling

[SNIPPY](https://github.com/tseemann/snippy) is a pipeline that contains different tools to determine SNPs in sequencing reads against a reference genome. It takes forward and reverse reads of paired-end sequences and aligns them against a reference. 

It is very fast but a single run of SNIPPY still takes about 10 to 12 minutes. We will therefore tell SNIPPY to run all the samples after each other. However this time we can not use a wildcard to do so. We will instead run all the samples in a loop.

~~~
for sample in ERR026473 ERR026474 ERR026478 ERR026481 ERR026482 ERR029206 ERR029207
  do
  snippy  --outdir ../results/snps/$sample --ref GCF_000195955.2_ASM19595v2_genomic.fna --R1 "$sample"_1.fastq.gz --R2 "$sample"_2.fastq.gz
  done
~~~
{: .bash}


**Detach session (process keeps running in background)**

You can detach from a session by pressing `control + a` followed by `d` (for detach) on your keyboard. 

You can now safely work on a different task or log out of your machine. The assembly will keep on running.



After the run is finished we can check the results.

~~~
head -n10 ../results/snps/ERR029207/ERR029207.tab 
~~~
{: .bash}

~~~
CHROM	POS	TYPE	REF	ALT	EVIDENCE	FTYPE	STRAND	NT_POS	AA_POS	EFFECT	LOCUS_TAG	GENE	PRODUCT
NC_000962.3	1849	snp	C	A	A:225 C:0								
NC_000962.3	1977	snp	A	G	G:130 A:0								
NC_000962.3	4013	snp	T	C	C:136 T:0								
NC_000962.3	7362	snp	G	C	C:141 G:0								
NC_000962.3	7585	snp	G	C	C:136 G:0								
NC_000962.3	9304	snp	G	A	A:128 G:0								
NC_000962.3	11820	snp	C	G	G:165 C:0								
NC_000962.3	11879	snp	A	G	G:107 A:0								
NC_000962.3	14785	snp	T	C	C:180 T:0	
~~~
{: .output} 

This list gives us information on every SNP that was found by SNIPPY when compared to the reference genome. The first SNP is found at the position 1849 of the reference genome, and is a C in the H37Rv and an A in isolate ERR029207. There is a high confidence associated with this SNP: an A has been found 225 times in the sequencing reads and never a C at this position. 



> ## Challenge: How many SNPs were identified in each sample??
>
> Find out how many SNPs were identified in the *M. tuberculosis* isolates when compared to H37Rv. Enter your solution in the
> [table](https://docs.google.com/spreadsheets/d/1xjiliy_USyMwiyzEgWhpn8_109F7Z3jPM_f7Jp-lOb8/edit?usp=sharing) under the head 'Number of SNPs'.
>
> Hint: The .txt file in the SNIPPY output contains summary information
> 
> > ## Solution
> >
> > 
> > ~~~
> > [1] head ../results/snps/ERR029207/ERR029207.txt
> >  
> > DateTime	2018-01-09T13:34:32
> > ReadFiles	/home/dcuser/dc_workshop/data/ERR029207_1.fastq.gz /home/dcuser/dc_workshop/data/ERR029207_2.fastq.gz
> > Reference	/home/dcuser/dc_workshop/data/GCF_000195955.2_ASM19595v2_genomic.fna
> > ReferenceSize	4485121
> > Software	snippy 3.2-dev
> > Variant-COMPLEX	29
> > Variant-DEL	54
> > Variant-INS	37
> > Variant-MNP	7
> > Variant-SNP	1292
> > 
> > This *M. tuberculosis* isolate contains 1292 SNPs compared to H37Rv.
> >
> > Repeat this for the other isolates.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

## Core SNPs

In order to compare the identified SNPs with each other we need to know if a certain position exists in all isolates.
A core site can have the same nucleotide in every sample (monomorphic) or some samples can be different (polymorphic). 
SNIPPY will concatenate the core sites, i.e. ignoring sites that are identical in all isolates and in the reference.

The '--no-ref' argument tells SNIPPY to exclude the reference from the alignment.  
The '--aformat' argument determines the alignment output format. We need a [newick format](https://en.wikipedia.org/wiki/Newick_format) for our further analysis, but SNIPPY only offers a range of other formats, including nexus. Format conversion is a [very common problem in bioinformatics](https://twitter.com/search?q=bioinformatics%20format%20conversion&src=typd) but luckily nexus to newick is one of the easier challenges.

~~~
cd ../results/snps/
snippy-core --no-ref --aformat=nexus ERR026473 ERR026474 ERR026478 ERR026481 ERR026482 ERR029206 ERR029207
~~~
{: .bash}


## Format conversion 

The output is in [Nexus format](https://en.wikipedia.org/wiki/Nexus_file). Let's view the output.

~~~
head core.nexus
~~~
{: .bash}

~~~

~~~
{: .output}

If we compare this to the [Newick format](https://en.wikipedia.org/wiki/Newick_format) it becomes clear that the Nexus file contains a tree annotation in Newick format. Because this phylogenetic tree contains only a limited amount of samples we can copy the Newick format out of the Nexus file and give it a new name: core_snps.newick


## Visualization of phylogenetic trees

We will be using [Dendroscope](http://dendroscope.org/) to view the phylogenetic tree established from the core SNPs. Dendroscope is a tree viewer which allows to edit the trees and zoom in and out.


> ## Challenge: Which isolates are related based on core SNPs??
> 
> Open Dendroscope on your own computer, go to File>Open and choose core_snps.newick
> Try to make three groups out of the 7 isolates.
> 
> > ## Solution
> >
> > 
> > ~~~
> > 3 groups:
> > ..
> > ..
> > ..
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

