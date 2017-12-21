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


## Assembly (over night)

Now we can start the loop with the assemblies

~~~
for sample in ERR026473 ERR026474 ERR026478 ERR026481 ERR026482 ERR029206 ERR029207
  do
  spades.py -1 "$sample"_R1_sub.fastq -2 "$sample"_R2_sub.fastq -o ../output/"$sample"
  done
~~~
{: .source}





{% include links.md %}
