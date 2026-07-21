---
title: "RNASeq: reads to differential genes and pathways"
type: "BioShell Training"
description: "Samaha G, Deshpande N, Lu C-Y, Chung J, Stott A, Ip A (2023)"
date: 2023
# participants:   # AUTHOR TO SUPPLY — number of people trained (shown in the community statistics)
doi: 10.5281/zenodo.10045628
materials_url: https://sydney-informatics-hub.github.io/rnaseq-workshop-2023/
---

RNA sequencing (RNAseq) is a popular and powerful technique used to understand the activity of genes. Using differential gene profiling methods, we can use RNAseq data to gain valuable insights into gene activity and identify variability in gene expression between samples to understand the molecular pathways underpinning many different traits.  

In this hands-on workshop, you will learn RNAseq fundamentals as you process, analyse, and interpret the results from a real RNAseq experiment on the command-line. In session one, you will convert raw sequence reads to analysis-ready count data with the nf-core/rnaseq workflow. In session two, you'll work interactively in RStudio to identify differentially expressed genes,perform functional enrichment analysis, and visualise and interpret your results using popular and best practice R packages.  

This workshop was delivered as a part of the Australian BioCommons Bring Your Own Data Platforms Project and will provide you with an opportunity to explore services and infrastructure built specifically for life scientists working at the command line. By the end of the workshop, you will be familiar with Pawsey's Nimbus cloud platform and be able to process your own RNAseq datasets and perform differential expression analysis on the command-line. 

Materials are shared under a Creative Commons Attribution 4.0 International agreement unless otherwise specified and were current at the time of the event.

{%- if page.participants %}

**Participants trained:** {{ page.participants }}
{%- endif %}

{%- if page.doi %}

**Citation**

> {{ page.description }}. **WORKSHOP: {{ page.title }}**. Zenodo.
> https://doi.org/{{ page.doi }}

{% if page.materials_url %}{% assign materials_link = page.materials_url %}{% else %}{% assign materials_link = page.doi | prepend: 'https://doi.org/' %}{% endif -%}
[**View training materials**]({{ materials_link }})
{%- endif %}

