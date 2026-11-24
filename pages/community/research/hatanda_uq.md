---
title: "Nextflow pipeline conversion to Galaxy workflow"
type: "BioShell Research"
description: "This project will explore the coversion of bioinformatics nf-core pipelines to a fully operational Galaxy workflow. I will use Agentic AI (claude code) and Galaxy-Foundry-written skills to support and enable this conversion."
researcher: "Atanda Halimat"
affiliation: "The University of Queensland" 
fields_of_research: [3102, 3101] 
---

{% include research-people.html %}

{% include research-fields.html %}

**Project:** {{ page.title }}

The aim is to explore how AI agents can support the translation of Nextflow pipelines into executable Galaxy workflows, to make them accessible to researchers who perfer a low/no-code environment. I will start with OncoAnalyser as an initial case study, and its successful conversion will add to the record of successes that will inform future translations.

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
