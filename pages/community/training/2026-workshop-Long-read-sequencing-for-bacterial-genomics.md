---
title: "Long read sequencing for bacterial genomics"
type: "BioShell Training"
description: "Antczak M, Geaghan M, Herne C, O'Brien MJ"   # short byline shown on the community tile
date: 2026                        # year the event ran (used for the statistics block)
participants: 24               # optional — people trained (counted in the statistics block)
doi: 10.5281/zenodo.23327650       # optional — Zenodo (or other) DOI, without the https://doi.org/ prefix
materials_url: https://sydney-informatics-hub.github.io/long-read-sequencing-workshop-2026/                      # optional — link to materials if not the DOI above
---

Short read sequencing has long been a default choice for many genomics studies, given its high accuracy. However, the restricted read length of 150-300bp limits its usefulness for genome assembly, particularly when trying to resolve large repetitive elements. In the field of bacterial genomics, accurate and complete genome assembly is key to distinguishing between genes present on the main chromosome versus mobile plasmids, which influence how elements like antimicrobial resistance genes spread.

The adoption of long read sequencing, with its multi-kilobase read length, has quickly risen in recent years to address these limitations. However, long read sequencing, particularly within the context of bacterial genomics, has its own set of challenges that require careful consideration.

This workshop will introduce you to the fundamentals of long read sequencing and how it can be used to assemble and analyse bacterial genomes. You will be guided through standard quality control analysis and pre-processing of long read sequencing data, assembling a bacterial genome, identifying plasmids, and annotating the genome to identify antimicrobial resistance genes of importance to hospital-acquired infections.

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
