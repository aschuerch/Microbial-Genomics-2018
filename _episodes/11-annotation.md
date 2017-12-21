---
title: "Annotation"
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

The contigs we assembled contain different genomic features. The process of identifying and labelling those features is called genome annotation.

Genome annotation includes prediction of protein-coding genes, as well as other functional genome units such as structural RNAs, tRNAs, small RNAs, pseudogenes, control regions, direct and inverted repeats, insertion sequences, transposons and other mobile elements. It starts by identifying open reading frames (ORFs). Predicted sequences are further analysed with BLAST


Open ERR029207.fna

~~~
more /home/dcuser/dc_workshop/output/ERR029207.fna 
~~~
{: .source}


Copy the first contig (fasta) by moving over it with the mouse and pressing the left mouse button to copy.

Go to [ORFfinder](https://www.ncbi.nlm.nih.gov/orffinder/)

Predict ORFs with translation table 11

Submit the longest ORF to BLAST

How would this ORF be annotated? Is it a gene or something else? What does the gene do?

Now we will annotate all genomes with an automated approach. Prokka is a pipeline script which coordinates a series of genome feature predictor tools and sequence similarity tools to annotate the genome sequence or contigs. Again, this will run for a while,

~~~
for sample in ERR026473 ERR026474 ERR026478 ERR026481 ERR026482 ERR029206 ERR029207
  do
  prokka --outdir anno_"$sample" --prefix prokka "$sample".fna
  done
~~~
{: .source}

Let's check the output:

~~~
cat ./anno*/prokka.txt
~~~
{: .source}



> ## Challenge: How many genes did Prokka find in the contigs??
>
> Find out how many genes there are in the *M. tuberculosis* isolates.
> 
> Hint:
> ~~~
> grep -c 
> ~~~
> prints a count of matching lines for each input file.
> 
> > ## Solution
> >
> > 
> > ~~~
> > [1] grep -c '>' *.fna
> > ..
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}



{% include links.md %}
