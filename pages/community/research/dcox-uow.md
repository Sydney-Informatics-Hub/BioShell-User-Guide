---
title: "Sequence-specific targeting of retroviral RNAs"
type: "BioShell Research"
description: "This project will deliver a versatile, scalable RNA-targeting workflow applicable to difficult or repetitive sequences. By exploiting the therapy-ready humanised CRISPR-Cas-Inspired RNA Targeting System (CIRTS) technology, this project will generate a validated toolbox for the design and screening of targeting RNAs at scale, primed for translation."
researcher: "Dezerae Cox"
affiliation: "University of Wollongong"
fields_of_research: [3101]   # TODO confirm against application (3101 or 3105?)
collaborators: "NSW RNA Research & Training Network"          # optional
---

{% include research-people.html %}

{% include research-fields.html %}

**Project:** {{ page.title }}

This project will deliver a versatile, scalable RNA-targeting workflow applicable to difficult or repetitive sequences. By exploiting the therapy-ready humanised CRISPR-Cas-Inspired RNA Targeting System (CIRTS) technology, this project will generate a validated toolbox for the design and screening of targeting RNAs at scale, primed for translation.

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
