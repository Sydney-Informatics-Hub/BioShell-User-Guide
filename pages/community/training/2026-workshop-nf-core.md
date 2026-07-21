---
title: "Unlocking nf-core: customising workflows for your research"
type: "BioShell Training"
description: "Willet C, Geaghan M, Gagalova K, Antczak M, Samaha G, O'Brien M (2026)"
date: 2026
participants: 25
doi: 10.5281/zenodo.20453332
materials_url: https://sydney-informatics-hub.github.io/customising-nfcore-workshop-2026/
---

This record includes training materials associated with the Australian BioCommons workshop ‘Unlocking nf-core: customising workflows for your research’. This workshop took place over two, 4 hour sessions on 27-28 May 2026.

Processing and analysing ‘omics datasets poses many challenges to life scientists, particularly when we need to share our methods with collaborators and scale up our research. Public and reproducible bioinformatics workflows, like those developed by nf-core, are therefore invaluable resources for the life science community.

nf-core is a community-driven effort to provide high-quality bioinformatics workflows for common analyses including, RNAseq, mapping, variant calling, and single cell transcriptomics. A big advantage of using nf-core workflows is the ability to customise and optimise them for different computational environments, types and sizes of data, and research goals. 

This workshop will set you up with the foundational knowledge required to run and customise nf-core workflows in a reproducible manner. Over two days you will learn about the nf-core tools utility, step through the code structure of nf-core workflows, and explore the various ways to adjust workflow parameters, customise processes, and configure workflows for your computational environment, using the nf-core/rnaseq workflow as a hands-on example.

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

