---
title: "Downloading data"
teaching: 10
exercises: 50
questions:
- "How can I organize my file system for a new bioinformatics project?"
- "How and where can data be downloaded?"
objectives:
- "Create a file system for a bioinformatics project."
- "Use 'for' loops to automate operations on multiple files"
keypoints:
- "Wget and Curl are two different ways to get data from the internet"
- "'for' loops let you perform the same set of operations on multiple files with a single command"
- "Sequencing data is large"
---

This episode describes how to retrieve all necessary data from public repositories - the raw sequenced data of our isolates and a reference genome. It also introduces for loops which we will use for the rest of our analysis.

## Preparation

To make room for our sequencing data we will remove the remaining sample data from the machine.

~~~
cd 
rm -r dc_sample_data/
rm .dc_sampledata_lite/
~~~
{: .source}

## Getting your project started

Project organization is one of the most important parts of a sequencing project, and yet is often overlooked amidst the
excitement of getting a first look at new data. Of course, while it's best to get yourself organized before you even begin your analyses,it's never too late to start, either.  

Genomics projects can quickly accumulate hundreds of files across 
tens of folders. Every computational analysis you perform over the course of your project is going to create
many files, which can especially become a problem when you'll inevitably want to run some of those
analyses again. For instance, you might have made significant headway into your project, but then have to remember the PCR conditions
you used to create your sequencing library months prior. 

Other questions might arise along the way: 
- What were your best alignment results?
- Which folder were they in: Analysis1, AnalysisRedone, or AnalysisRedone2?
- Which quality cutoff did you use?
- What version of a given program did you implement your analysis in?

In this exercise we will setup a file system for the project we will be working on during this workshop.  

We will start by creating a directory that we can use for the rest of the workshop. First navigate to your home directory. Then confirm that you are in the correct directory using the `pwd` command.

~~~
$ cd
$ pwd
~~~
{: .bash}

You should see the output: 

~~~
/home/dcuser  
~~~
{: .output}

> ## Tip  
> If you aren't in your home directory, the easiest way to get there is to enter the command `cd`, which
> always returns you to home.  
{: .callout}

> ## Exercise  
> Use the `mkdir` command to make the following directories:   
> dc_workshop     
> dc_workshop/docs  
> dc_workshop/data    
> dc_workshop/results   
> 
> > ## Solution
> > 
> > ~~~
> > $ mkdir dc_workshop
> > $ mkdir dc_workshop/docs
> > $ mkdir dc_workshop/data
> > $ mkdir dc_workshop/results
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

Use `ls -R` to verify that you have created these directories. The `-R` option for `ls` stands for recursive. This option causes
`ls` to return the contents of each subdirectory within the directory
iteratively. 

~~~
$ ls -R dc_workshop
~~~
{: .bash}

You should see the following output:

~~~
dc_workshop/:
data  docs  results

dc_workshop/data:

dc_workshop/docs:

dc_workshop/results: 
~~~
{: .output}



## Selection of a reference genome

Reference sequences (including many pathogen genomes) are available at [NCBI's refseq database](https://www.ncbi.nlm.nih.gov/refseq/)

A reference genome is a genome that was previously sequenced and is closely related to the isolates we would like to analyse. The selection of a closely related reference genome is not trivial and can warrant an analysis in itself. However, for simplicity, here we will work with the *M. tuberculosis* reference genome H37Rv.

### Download reference genomes from NCBI

Download the *M.tuberculosis* reference genome from the NCBI ftp site.

First, we switch to the data folder to store all our data

~~~
$ cd dc_workshop/data 
~~~
{: .source}


With curl we use the -O flag, which simultaneously tells curl to download the page instead of showing it to us and specifies that it should save the file using the same name it had on the server:

~~~
$ curl -O ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/195/955/GCF_000195955.2_ASM19595v2/GCF_000195955.2_ASM19595v2_genomic.fna.gz
~~~
{: .source}


This file is compressed (gunzipped) as indicated by the extension of ".gz". It means that this file has been compressed using the "gzip" command.

Extract the file by typing

~~~
$ gunzip GCF_000195955.2_ASM19595v2_genomic.fna.gz
~~~
{: .source}

Make sure that is was compressed

~~~
$ ls
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
> $ grep pattern [filename]
> ~~~
> selects lines in files that match patterns
>

> > ## Solution
> >
> > 
> > ~~~
> > $ grep -v '>' GCF_000195955.2_ASM19595v2_genomic.fna| wc -m
> > 4466677
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}


There are a few ways to keep cloud processes running in the background. Many times when we refer to a background process we are talking about what is described at this tutorial - running a command and returning to shell prompt. Here we describe a program that will allow us to run our entire shell and keep that process running even if we disconnect.

## Starting and attaching to screen sessions

### Starting a new session

A ‘session’ can be thought of as a window for screen, you might open an terminal to do one thing on the a computer and then open a new terminal to work on another task at the command line. You can start a session and give it a descriptive name:

~~~
$ screen -S session_name
~~~
{: .source}

This creates a session with the name ‘session_name’

As you work, this session will stay active until you close this session. Even if you disconnect from your machine, the jobs you start in this session will run till completion.


## Download the sequenced genomes from the European Nucleotide Archive (ENA)

`wget` is short for “world wide web get”, and it’s basic function is to download web pages or data at a web address.
There are many repositories for public data. Some model organisms or fields have specific databases, and there are ones for particular types of data. Two of the most comprehensive are the National Center for Biotechnology Information (NCBI) and European Nucleotide Archive (EMBL-EBI). In this lesson we’re working with the ENA, but the general process is the same for any database.

Let's download our *M. tuberculosis* data with

~~~
$ wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR029/ERR029207/*fastq.gz
$ wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR029/ERR029206/*fastq.gz
$ wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026478/*fastq.gz

$ wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026474/*fastq.gz
$ wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026473/*fastq.gz

$ wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026481/*fastq.gz
$ wget ftp://ftp.sra.ebi.ac.uk/vol1/fastq/ERR026/ERR026482/*fastq.gz
~~~
{: .source}

This will take a moment and might run over night. We will therefore detach the session to work further.

**Detach session (process keeps running in background)**

You can detach from a session by pressing `control + a` followed by `d` (for detach) on your keyboard. 

You can now safely log out from your machine. The assembly will run over night.

If you reconnect the next day to your machine, you will also have to reconnect to your session to see how it went.


> ## Additional session commands
> **Seeing active sessions**
> If you disconnect from your session, or from your ssh into a machine, you will need to reconnect to an existing 
> `screen` session. You can see a list of existing sessions:
> ~~~
> $ screen -ls
> ~~~
> {: .bash}
> **Reconnecting to a session**
> To reconnect to an existing session:
> 
> ~~~
> $ screen -r session_name
> ~~~
> {: .bash}
> 
> The `-r` option = 'resume  a detached screen session'
>
> **Kill a session**
> To end a session, type `exit` after reconnecting to the session:
> 
> ~~~
> $ screen -r session_name
> $ exit
> ~~~
> {: .bash}
{: .callout}

After the download finished, let's have a look at the files

~~~
$ ls
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


{% include links.md %}




