---
title: Intro to Nextflow (rerun)
type: "BioShell Training"
description: "Lupat R and Li S (2026)"
date: 2026
participants: 29 
# doi: <10.5281/zenodo.xxxxxxxx>       # optional — Zenodo (or other) DOI, without the https://doi.org/ prefix
materials_url: https://sydney-informatics-hub.github.io/nf4ls-materials/
---



The first session provides participants with the core concepts of Nextflow pipeline development and foundational knowledge needed to begin building and customising workflows. In the second session we will step through the hands-on practice in creating a scalable multi-sample Nextflow workflow, consisting of multiple bioinformatics tools.

This workshop was rerun by Peter Mac using a BioShell image saved in our workshop library. The content was trained and delivered using the [**Nextflow for the Life Sciences train-the-trainer guide and workshop materials**](https://sydney-informatics-hub.github.io/nf4ls-materials/).

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
