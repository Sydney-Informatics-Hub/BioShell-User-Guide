---
title: "Spatial sampler"
type: "BioShell Research"
description: "Spatial Sampler is a collection of practical analysis recipes designed to answer specific biological questions using spatial omics data. This project enables reproducible testing and validation of Spatial Sampler analyses on BioShell images."
researcher: "Sarah Williams" 
affiliation: "Queensland Cyber Infrastructure Foundation (QCIF)"

fields_of_research: [3102, 3101] 

---

{% include research-people.html %}

{% include research-fields.html %}

**Project:** {{ page.title }}

[Spatial Sampler](https://swbioinf.github.io/spatialsnippets/index.html) is a collection of practical "How-To" guides for spatial omics data analysis, organised around answering specific biological questions through comparative testing between experimental conditions. Each workflow provides a worked example, required inputs, expected outputs, and reusable code snippets that can be applied to new datasets.

This project enables reproducible testing and validation of Spatial Sampler analyses on BioShell images.

{%- if page.collaborators or page.funding %}

**Collaborators and funding**

{% if page.collaborators %}
- **Collaborators:** {{ page.collaborators }}
{%- endif %}
{%- if page.funding %}
- **Funding:** {{ page.funding }}
{%- endif %}
{%- endif %}

{%- if page.doi %}

[**View publication**](https://doi.org/{{ page.doi }})
{%- endif %}
