---
title: "Sequence assembly"
teaching: 45
exercises: 30
questions:
- "How can the information in the sequencing reads be reduced?"
- "What are the different methods for assembly?"
objectives:
- "Understand differences between assembly methods"
- "Assemble the short reads"
- "Interpret an assembly QC plot"
---

Sequence assembly means the alignment and merging of reads in order to reconstruct the original sequence.

The problem of sequence assembly can be compared to taking many copies of a book, passing each of them through a shredder with a different cutter, and piecing the text of the book back together just by looking at the shredded pieces. Besides the obvious difficulty of this task, there are some extra practical issues: the original may have many repeated paragraphs, and some shreds may be modified during shredding to have typos. Excerpts from another book may also be added in, and some shreds may be completely unrecognizable.

Different assembly algorithms are explained, but we are going to use a de Bruijn graph method to assemble our genome with [SPAdes](http://bioinf.spbau.ru/spades).

The parameter of the SPAdes mean the following:

`-1` is a file with forward paired-end reads
`-2` is a file with reverse paired-end reads
`-o` denote the directory to store all the resulting files

In order to run all assemblies, we will tell the computer to process this in a loop. This will take a long time and it is best to run this overnight. Therefore we will start a screen session which will not be killed if you disconnect your machine at the end of the day.
There are a few ways to keep cloud processes running in the background. Many times when we refer to a background process we are talking about what is described at this tutorial - running a command and returning to shell prompt. Here we describe a program that will allow us to run our entire shell and keep that process running even if we disconnect.

## Starting and attaching to screen sessions

### Starting a new session

A ‘session’ can be thought of as a window for screen, you might open an terminal to do one thing on the a computer and then open a new terminal to work on another task at the command line. You can start a session and give it a descriptive name:

~~~
screen -S session_name
~~~
{: .source}

This creates a session with the name ‘session_name’

As you work, this session will stay active until you close this session. Even if you disconnect from your machine, the jobs you start in this session will run till completion.


Now we can start the loop with the assemblies

~~~
for sample in samples
  do
  spades.py -1 "$sample"_R1_sub.fastq -2 "$sample"_R2_sub.fastq -o ../output/"$sample"
  done
~~~
{: .source}





{% include links.md %}
