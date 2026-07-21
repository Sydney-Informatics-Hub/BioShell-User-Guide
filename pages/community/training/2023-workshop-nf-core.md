---
title: "Unlocking nf-core: customising workflows for your research"
type: "BioShell Training"
description: "Samaha G, Willet C, Hakkaart C, Beecroft S, Stott A, Ip A, Cooke S (2023)"
date: 2023
participants: 37
doi: 10.5281/zenodo.8026170
materials_url: https://sydney-informatics-hub.github.io/customising-nfcore-workshop/ 
---

Processing and analysing omics datasets poses many challenges to life scientists, particularly when we need to share our methods with other researchers and scale up our research. Public and reproducible bioinformatics workflows, like those developed by nf-core, are invaluable resources for the life science community.

nf-core is a community-driven effort to provide high-quality bioinformatics workflows for common analyses including, RNAseq, mapping, variant calling, and single cell transcriptomics. A big advantage of using nf-core workflows is the ability to customise and optimise them for different computational environments, types and sizes of data and research goals. 

This workshop will set you up with the foundational knowledge required to run and customise nf-core workflows in a reproducible manner. On day 1 you will learn about the nf-core tools utility, and step through the code structure of nf-core workflows. Then on day 2, using the nf-core/rnaseq workflow as an example, you will explore the various ways to adjust the workflow parameters, customise processes, and configure the workflow for your computational environment.

This workshop event and accompanying materials were developed by the Sydney Informatics Hub, University of Sydney in partnership with Seqera Labs, Pawsey Supercomputing Research Centre, and Australia’s National Research Education Network (AARNet). The workshop was enabled through the Australian BioCommons - Bring Your Own Data Platforms project (Australian Research Data Commons and NCRIS via Bioplatforms Australia). 

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

