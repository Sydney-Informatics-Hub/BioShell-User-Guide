---
title: "Spatial omics sampler"
type: "BioShell Training"
description: "Williams S (2026)"
date: 2026
# participants:   # AUTHOR TO SUPPLY — number of people trained (shown in the community statistics)
doi: 10.5281/zenodo.18795925
materials_url: https://swbioinf.github.io/intro-spatial-transcriptomics-workshop/index.html
---

Spatial omics provides unprecedented opportunities for the understanding of cells, tissues and systems by combining omics and imaging technologies.

This hands-on workshop will show you how to analyse data from in situ spatial (e.g. CosMx, Xenium) experiments with Seurat in R to visualise gene expression within tissue samples. Using real-life experimental data we step through the process of reading in data, quality control, filtering, dimensionality reduction, visualisation and differential expression analysis. We will discuss the ‘why’ behind each step and essential best practices for designing and running spatial omics experiments.

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

