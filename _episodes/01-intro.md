---
title: "Introduction"
teaching: 10
exercises: 0
questions:
- "What are the goals of this practical training?"
- "What data do we work on?"
objectives:
- "Describe the goals of this practical training."
- "Describe the characteristics of the isolates."
keypoints:
- "We will work towards confirming or disputing transmission in TB cases"
- "After this practical training you will have some familiarity with working on the command line"
---

## Working with the command line

In order to work with high-throughput sequencing data, it is necessary to have some knowledge of the command line, because:

   + Sequencing files are large and can more efficiently and quickly be handled by text-based commands
   + Most (bioinformatic) tools for sequencing data are written to be used from the command line, without a graphical interface
   + To use remote computers (where most calculations take place) you need the command line

The first goal is therefore to learn a number of basic operations in the shell which can be applied when working with sequencing data. This knowledge will be used when analysing the data with bioinformatic command line tools.



## Background of the data

Epidemiological contact tracing through interviewing of patients can identify potential chains of patients that transmitted the disease to each other. Here we have 7 tuberculosis patients that form 3 separate clusters as determined by epidemiological interviews. Patients were asked if they have been in direct contact with each other, or if they visited the same localities. From all patients, a bacterial isolate was grown, DNA isolated, and whole-genome sequenced on an Illumina sequencer: 

    - Patient A1 - sample ERR029207
    - Patient A2 - sample ERR029206
    - Patient A3 - sample ERR026478


    - Patient B1 - sample ERR026474
    - Patient B2 - sample ERR026473


    - Patient C1 - sample ERR026481
    - Patient C2 - sample ERR026482

The second goal of this practicum is to affirm or dispute the inferred transmissions by comparing the bacterial genomes with each other. We will do that by two strategies: by identifying SNPs between the genomes and by identifying variably present genes, and we will compare the results of the two methods.



{% include links.md %}
