---
title: "Annotation"
start: true
teaching: 20
exercises: 90
questions:
- "How are proteins predicted from a DNA sequence?"
objectives:
- "Search for open reading frames"
- "Predict a protein from an open reading frame"
- "Annotate a genome"
keypoints:
- "Genome annotation includes prediction of protein-coding genes, as well as other functional genome units"
- "It often starts by identifying open reading frames"
- "Predicted sequences are further analysed with BLAST"
- "Larger DNA sequences or genomes require automated prediction and annotation"
---

Now that we have assembled the data into contigs the next natural step to do is annotation of the data. The contigs we assembled contain different genomic features. The process of identifying and labelling those features is called genome annotation.

Genome annotation includes prediction of protein-coding genes, as well as other functional genome units such as structural RNAs, tRNAs, small RNAs, pseudogenes, control regions, direct and inverted repeats, insertion sequences, transposons and other mobile elements. It starts by identifying open reading frames (ORFs). Predicted sequences are further analysed to search for similarity to known elements, for example with BLAST.


Open ERR029207.fna

~~~
head -n50 /home/dcuser/dc_workshop/results/assembly/ERR029207.fasta
~~~
{: .source}


Copy the first contig (fasta) by moving over it with the mouse and pressing the left mouse button to copy.

Go to [ORFfinder](https://www.ncbi.nlm.nih.gov/orffinder/). Paste the sequence including the header into the query field. Choose the genetic code (translation table) 11. Start the search by pressing 'submit'. This will open the Open Reading Frame viewer.

Search for the longest ORF. If you have found it, click on 'Mark'. Submit the longest ORF to BLAST.

How would this ORF be annotated? Is it a gene or something else? What does the gene do? Fill your annotation into the [table](https://docs.google.com/spreadsheets/d/1xjiliy_USyMwiyzEgWhpn8_109F7Z3jPM_f7Jp-lOb8/edit?usp=sharing).

Now we will annotate all genomes with an automated approach. Prokka is a pipeline script which coordinates a series of genome feature predictor tools and sequence similarity tools to annotate the genome sequence or contigs. 
A range of programs are available for these tasks but here we will use PROKKA, which is a pipeline comprising several open source bioinformatic tools and databases.

PROKKA automates the process of locating ORFs and RNA regions on contigs, translating ORFs to protein sequences, searching for protein homologs and producing standard output files. For gene finding and translation, PROKKA makes use of the program Prodigal. Homology searching (via BLAST and HMMER) is then performed using the translated protein sequences as queries against a set of public databases (CDD, PFAM, TIGRFAM) as well as custom databases that come with PROKKA.

The names of the contigs produced by SPades are quite long. PROKKA needs name which are shorter than 20 characters. We will therefore shorten these names first by making a copy of the contig files.

~~~
cd ~/dc_workshop/results/assembly/
for sample in ERR026473 ERR026474 ERR026478 ERR026481 ERR026482 ERR029206 ERR029207
  do
  cut -f 1,8 -d "_" "$sample"/contigs.fasta | sed s/NODE/C/g > "$sample".fasta
  done
~~~

Let's see what this has done. First let's have a look at the original files.

~~~
head -n10 ERR026473/contigs.fasta
~~~
{: .bash}

~~~
>NODE_1_length_43202_cov_4.41729_ID_34907
TAGCTTGTCGAGCGTGCGGGGGCTTATGCGTCTGCTCGCCCTAGAGACTGAGCAAGATCT
CCCGGGCCAGCGCCGAGGTCGCCGATGGGGTCTTGCCGACCTTCACACCGGCGGCCTCCA
GGGCCTCTTGCTTGGCCGCCGCTGTGCCAGACGAGCCGGAGACGATGGCGCCGGCGTGGC
CCATCGTCTTGCCTTCGGGTGCGGTAAATCCGGCGACATAGCCGACGACCGGCTTGGACA
CGTTGGTCTTGATGAAGTCTGCGGCCCGCTCCTCGGCGTCACCACCGATCTCGCCGATCA
TCACGATGAGCTTGGTGTCCGGATCCCTCTCGAAGGCCTCGATGGCGTCGATGTGGGTAG
TGCCAATCACCGGATCACCACCGATGCCGATCGCCGTGGAGAATCCAAGGTCGCGCAGTT
CGAACATCATCTGGTAGGTCAACGTCCCCGACTTGGACACCAGACCAATTGGACCGGGTC
CGGTGATGTTGGCCGGCGTGATACCGGCCAGCGACTGACCGGGACTGATAATGCCAGGAC
~~~
{: .output}

Now, let's inspect the new file

~~~
head -n10 ERR026473.fasta
~~~
{: .bash}

~~~
>C_34907
TAGCTTGTCGAGCGTGCGGGGGCTTATGCGTCTGCTCGCCCTAGAGACTGAGCAAGATCT
CCCGGGCCAGCGCCGAGGTCGCCGATGGGGTCTTGCCGACCTTCACACCGGCGGCCTCCA
GGGCCTCTTGCTTGGCCGCCGCTGTGCCAGACGAGCCGGAGACGATGGCGCCGGCGTGGC
CCATCGTCTTGCCTTCGGGTGCGGTAAATCCGGCGACATAGCCGACGACCGGCTTGGACA
CGTTGGTCTTGATGAAGTCTGCGGCCCGCTCCTCGGCGTCACCACCGATCTCGCCGATCA
TCACGATGAGCTTGGTGTCCGGATCCCTCTCGAAGGCCTCGATGGCGTCGATGTGGGTAG
TGCCAATCACCGGATCACCACCGATGCCGATCGCCGTGGAGAATCCAAGGTCGCGCAGTT
CGAACATCATCTGGTAGGTCAACGTCCCCGACTTGGACACCAGACCAATTGGACCGGGTC
CGGTGATGTTGGCCGGCGTGATACCGGCCAGCGACTGACCGGGACTGATAATGCCAGGAC
~~~
{: .output}

The header was shortened to less than 20 characters. It is important that each header is still unique. We can check this by inspecting a few headers.

~~~
grep  ">" ERR026473.fasta
~~~
{: .bash}

~~~
>C_36541
>C_36543
>C_36545
>C_36547
>C_36549
>C_36551
>C_36553
>C_36555
>C_36557
>C_36559
>C_36561
>C_36563
>C_36565
>C_36567
>C_36569
>C_36571
>C_36573
>C_36575
>C_36577
>C_36579
>C_36581
~~~
{: .output}

These are the last few lines of our output. Every line has a unique number.

We still need to make a new folder to contain our annotation results.

~~~
cd ~/dc_workshop/results
mkdir annotation
~~~
{: .bash}

Now we are all set to annotate our contigs with PROKKA. Again, this will run for a while.
The parameter --outdir tells PROKKA which output directory to write to. This needs to be a new file. 
The parameter --prefix assigns the sample name as a prefix to all files. If we ommit this, all files will have the same names

~~~
for sample in ERR026473 ERR026474 ERR026478 ERR026481 ERR026482 ERR029206 ERR029207
  do
  prokka --outdir annotation/anno_"$sample" --prefix $sample assembly/"$sample".fasta
  done
~~~
{: .bash}

Let's check the output:

~~~
cat annotation/anno_*/ERR*.txt
~~~
{: .bash}


> ## Exercise: How many coding regions did PROKKA find in the contigs??
>
> Find out how many coding regions there are in the *M. tuberculosis* isolates. Enter your solution in the
> [table](https://docs.google.com/spreadsheets/d/1xjiliy_USyMwiyzEgWhpn8_109F7Z3jPM_f7Jp-lOb8/edit?usp=sharing) under the head 'Number of CDS'
> Hint:
> ~~~
> grep "CDS" 
> ~~~
> prints a count of matching lines for each input file.
> 
> > ## Solution
> >
> > 
> > ~~~
> > [1] grep CDS annotation/anno_*/ERR*.txt
> >  
> > annotation/anno_ERR026473/prokka.txt:CDS: 4336
> > annotation/anno_ERR026474/prokka.txt:CDS: 4179
> > annotation/anno_ERR026478/prokka.txt:CDS: 4175
> > annotation/anno_ERR026481/prokka.txt:CDS: 4154
> > annotation/anno_ERR026482/prokka.txt:CDS: 4125
> > annotation/anno_ERR029206/prokka.txt:CDS: 4182
> > annotation/anno_ERR029207/prokka.txt:CDS: 4138
> > 
> > These *M. tuberculosis* genomes contain between 4125 and 4336 coding regions.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

Repeat the same exercise with the key word 'bases' to see how big the genomes are and fill into the [table](https://docs.google.com/spreadsheets/d/1xjiliy_USyMwiyzEgWhpn8_109F7Z3jPM_f7Jp-lOb8/edit?usp=sharing) under the head 'Genome size'

{% include links.md %}
