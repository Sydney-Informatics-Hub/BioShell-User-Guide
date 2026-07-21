---
title: "Nextflow for the life sciences"
type: "BioShell Training"
description: "Jaya F, Geaghan M, Xue W, Antczak M, Gauthier M-E, Beecroft S, O'Brien M, et al (2025)"
date: 2025
participants: 53
doi: 10.5281/zenodo.16791039 
materials_url: https://sydney-informatics-hub.github.io/hello-nextflow-2025/
---

The rise of big data has made it essential to be able to analyse and perform experiments on large datasets in a portable and reproducible manner. Nextflow is a popular bioinformatics workflow orchestrator that makes it easy to run data-intensive computational pipelines. It enables scalable and reproducible scientific workflows using software containers on any infrastructure. It allows the adaptation of workflows written in most languages and provides the ability to customise and optimise workflows for different computational environments, types and sizes of data.

Learn to build reproducible and scalable scientific workflows with Nextflow in this two-part workshop. Part one covers the fundamental principles of Nextflow pipeline development using the “Hello Nextflow” materials. Part two offers practical, hands-on experience creating a multi-sample Nextflow workflow for RNAseq data preparation

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

