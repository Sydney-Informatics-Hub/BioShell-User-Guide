---
title: "Single cell RNAseq analysis in R"
type: "BioShell Training"
description: "Williams S, Jaya F, Barugahare A, Harrison P (2026)"   # short byline shown on the community tile
date: 2026
participants: 29 
doi: 10.5281/zenodo.21784360 
materials_url: https://swbioinf.github.io/scRNAseq_Workshop/
---

Analysis and interpretation of single cell RNAseq (scRNAseq) data requires dedicated workflows. In this hands-on workshop we will show you how to perform single cell RNAseq analysis using Seurat, Harmony and Single R - R packages for QC, analysis, and exploration of single-cell RNAseq data. 

We will discuss the ‘why’ behind each step and cover reading in the count data, quality control, filtering, normalisation, clustering, UMAP layout and identification of cluster markers. We will also explore various ways of visualising single cell expression data.

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

<!--
HOW TO USE THIS TEMPLATE
1. Copy this file into pages/community/training/ and rename it to
   <year>-workshop-<short-slug>.md (the basename becomes the page URL, so keep it unique
   across the whole site).
2. Fill in the front matter. Only title, type, description and date are required;
   participants, doi and materials_url are optional — delete any you don't use.
3. Write the event description, then delete this comment block.
The tile and the statistics block on the community page are generated automatically from
any page with type: "BioShell Training" — no need to edit community.md itself.
-->
