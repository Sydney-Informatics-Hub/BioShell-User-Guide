---
title: "Spatial omics"
type: "BioShell Training"
description: "Jaya F, Williams S, O'Brien MJ, Matigian N, Thind AS, Wang C, Mori G (2025)"
date: 2025
# participants:   # AUTHOR TO SUPPLY — number of people trained (shown in the community statistics)
doi: 10.5281/zenodo.18168944       # optional — Zenodo (or other) DOI, without the https://doi.org/ prefix
materials_url: https://swbioinf.github.io/spatial-taster-workshop/
---

This workshop provides a practical introduction to comparative analysis of spatial omics data. Starting with a pre-processed in situ spatial dataset, we will undertake some basic differential expression, look at differences of proportions of cell types, and exploratory plotting. We will work in R, using mostly Bioconductor tools. We will use a cosMx dataset, but these approaches are applicable to other technologies like Xenium or vizgen.

Along the way we will discuss considerations specific to spatial analyses, how to scale it up to a full analysis, and how to assess our results. We will also introduce the Spatial Sampler collection of worked examples and code snippets for analyses on spatial datasets.

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

