---
title: "Genetic Outlier Analysis"
type: "BioShell Training"
description: "Stuart K, Samaha G, Barugahare A, Miller S, Deshpande N, Lu C-Y (2024)"
date: 2024
# participants:   # AUTHOR TO SUPPLY — number of people trained (shown in the community statistics)
doi: 10.5281/zenodo.14676276       # optional — Zenodo (or other) DOI, without the https://doi.org/ prefix
materials_url: https://genomicsaotearoa.github.io/Outlier_Analysis_Workshop/
---

There are many interesting patterns that you can extract from genetic variant data. This can include patterns of linkage, balancing selection, or even inbreeding signals. One of the most common approaches is to find sites on the genome that are under selection. 

This workshop introduces the basics of genetic selection analysis. It will step you through the process of identifying signals of selection using your own data (or an example genomic dataset) using the outlier analysis method. 

Materials are shared under a Creative Commons Attribution 4.0 International agreement unless otherwise specified and were current at the time of the event.

This workshop is presented by the Australian BioCommons and the Genetics Society of AustralAsia

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

