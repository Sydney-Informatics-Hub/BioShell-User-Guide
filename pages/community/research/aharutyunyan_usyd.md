---
title: "Mapping the co-aggregation proteome of TDP-43 in models of ALS/FTD"
type: "BioShell Research"
description: "We aim to understand the molecular and cellular causes of amyotrophic lateral sclerosis (ALS)/motor neuron disease (MND) and frontotemporal dementia (FTD). To this end we employ various genetically engineered in vitro and in vivo model systems, advanced microscopy and biochemistry techniques to develop and test potential therapeutics in proof-of-concept pre-clinical studies."
researcher: "Anna Harutyunyan"
affiliation: "The University of Sydney"
fields_of_research: [3101, 3209, 3102]   # TODO confirm against application
collaborators: "Neurodegeneration Pathobiology Group; Charles Perkins Centre, Sydney Pharmacy School and Centre for Drug Discovery and Innovation, The University of Sydney"
funding: "NHMRC, FightMND"
---

{% include research-people.html %}

{% include research-fields.html %}

**Project:** {{ page.title }}

We aim to understand the molecular and cellular causes of amyotrophic lateral sclerosis (ALS)/motor neuron disease (MND) and frontotemporal dementia (FTD). To this end we employ various genetically engineered in vitro and in vivo model systems, advanced microscopy and biochemistry techniques to develop and test potential therapeutics in proof-of-concept pre-clinical studies.

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
