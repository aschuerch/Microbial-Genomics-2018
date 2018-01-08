---
title: "Downloading data"
teaching: 10
exercises: 50
questions:
- "How and where can data be downloaded?"
objectives:
- "Prepare for downstream analysis"
keypoints:
- "Wget and Curl are two different ways to get data from the internet"
- "Sequencing data is large"
---

This episode describes how to retrieve all necessary data from public repositories - the raw sequenced data of our isolates and a reference genome.

## Preparation

To make room for our sequencing data we will remove the remaining sample data from the machine.

~~~
cd 
rm -r dc_sample_data/
rm .dc_sampledata_lite/
~~~
{: .source}

## Selection of a reference genome

Reference sequences (including many pathogen genomes) are available at [NCBI's refseq database](https://www.ncbi.nlm.nih.gov/refseq/)

A reference genome is a genome that was previously sequenced and is closely related to the isolates we would like to analyse. The selection of a closely related reference genome is not trivial and can warrant an analysis in itself. However, for simplicity, here we will work with the *M. tuberculosis* reference genome H37Rv.

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


This file is compressed (gunzipped) as indicated by the extension of ".gz". It means that this file has been compressed using the "gzip" command.

Extract the file by typing

~~~
gunzip GCF_000195955.2_ASM19595v2_genomic.fna.gz
~~~
{: .source}

Make sure that is was compressed

~~~
ls
~~~
{: .source}

~~~
GCF_000195955.2_ASM19595v2_genomic.fna
~~~
{: .output}


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

This will take a moment. After the download finished, let's have a look at the files

~~~
ls
~~~
{: .source}

~~~
ERR026473_1.fastq.gz  ERR026478_1.fastq.gz  ERR026482_1.fastq.gz  ERR029207_1.fastq.gz
ERR026473_2.fastq.gz  ERR026478_2.fastq.gz  ERR026482_2.fastq.gz  ERR029207_2.fastq.gz
ERR026474_1.fastq.gz  ERR026481_1.fastq.gz  ERR029206_1.fastq.gz  GCF_000195955.2_ASM19595v2_genomic.fna
ERR026474_2.fastq.gz  ERR026481_2.fastq.gz  ERR029206_2.fastq.gz
~~~
{: .output}

All new files start with ERR and have an fastq.gz extension. It means they are in fastq format and compressed. Let's uncompress with a  `for` loop to iterate through all of
our `.gz` files. Let's see what that looks like and then we'll 
discuss what we're doing with each line of our loop.

~~~
$ for filename in *.gz
> do
> gunzip $filename
> done
~~~
{: .bash}

When the shell sees the keyword `for`,
it knows to repeat a command (or group of commands) once for each item in a list.
Each time the loop runs (called an iteration), an item in the list is assigned in sequence to
the **variable**, and the commands inside the loop are executed, before moving on to 
the next item in the list.

Inside the loop,
we call for the variable's value by putting `$` in front of it.
The `$` tells the shell interpreter to treat
the **variable** as a variable name and substitute its value in its place,
rather than treat it as text or an external command. 

In this example, the list is fourteen filenames (one filename for each of our `.gz` files).
Each time the loop iterates, it will assign a file name to the variable `filename`
and run the `gunzip` command.
The first time through the loop,
`$filename` is `ERR026473_1.fastq.gz`. 
The interpreter runs the command `unzip` on `ERR026473_1.fastq.gz`.
For the second iteration, `$filename` becomes 
`ERR026473_2.fastq.gz`. This time, the shell runs `gunzip` on `ERR026473_2.fastq.gz`.
It then repeats this process for the four other `.gz` files in our directory.

> ## Follow the Prompt
>
> The shell prompt changes from `$` to `>` and back again as we were
> typing in our loop. The second prompt, `>`, is different to remind
> us that we haven't finished typing a complete command yet. A semicolon, `;`,
> can be used to separate two commands written on a single line.
{: .callout}

> ## Same Symbols, Different Meanings
>
> Here we see `>` being used a shell prompt, whereas `>` is also
> used to redirect output.
> Similarly, `$` is used as a shell prompt, but, as we saw earlier,
> it is also used to ask the shell to get the value of a variable.
>
> If the *shell* prints `>` or `$` then it expects you to type something,
> and the symbol is a prompt.
>
> If *you* type `>` or `$` yourself, it is an instruction from you that
> the shell to redirect output or get the value of a variable.
{: .callout}

We have called the variable in this loop `filename`
in order to make its purpose clearer to human readers.
The shell itself doesn't care what the variable is called;
if we wrote this loop as:

~~~
$ for x in *.gz
> do
> gunzip $x
> done
~~~
{: .bash}

or:

~~~
$ for temperature in *.gz
> do
> gunzip $temperature
> done
~~~
{: .bash}

it would work exactly the same way.
*Don't do this.*
Programs are only useful if people can understand them,
so meaningless names (like `x`) or misleading names (like `temperature`)
increase the odds that the program won't do what its readers think it does.

> ## Multipart commands
> The `for` loop is interpreted as a multipart command.  If you press the up arrow on your keyboard to recall the command, it will be shown like so:
>
> ~~~   
> $ for filename in *.gz; do echo File $gz; unzip $gz; done
> ~~~
> {: .bash}
> 
> When you check your history later, it will help your remember what you did!
>
{: .callout}

When we run our `for` loop, you will see output that starts like this:

~~~
.. 
~~~
{: .output}



> ## Alternative download
> If the download takes too long we can instead download a smaller version of the dataset from an earlier version of 
> this tutorial 
> 
> ~~~
> cd /home/dcuser/dc_workshop/data
> git clone https://github.com/aschuerch/MolEpiCourse.git
> mv MolEpiCourse/*fastq .
> rm -rf MolEpiCourse
> ~~~
> {: .bash}
{: .callout}


{% include links.md %}




