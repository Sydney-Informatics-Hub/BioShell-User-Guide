---
title: "Analysis of data for prediction of cancer patient response to checkpoint therapy"
type: "BioShell Research"
description: "This project investigates multi-omic biomarkers that provide robust, reproducible prediction of a cancer patient's likelihood of responding to immune checkpoint blockade. By integrating bulk RNA-seq and ATAC-seq with flow-cytometric immune subset profiling, we aim to uncover the transcriptomic and epigenetic determinants of primary immunotherapy resistance across lung cancer and melanoma cohorts."
researcher: "Justin Yang"   # counted in the statistics block
affiliation: "The University of Sydney"      # counted as an institution in the statistics block
collaborators: "Chris O'Brien Lifehouse "          # optional
---

{% include research-people.html %}

**Project:** {{ page.title }}

This project investigates multi-omic biomarkers that provide robust, reproducible prediction of a cancer patient's likelihood of responding to immune checkpoint blockade. By integrating bulk RNA-seq and ATAC-seq with flow-cytometric immune subset profiling, we aim to uncover the transcriptomic and epigenetic determinants of primary immunotherapy resistance across lung cancer and melanoma cohorts.

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

<!--
HOW TO USE THIS TEMPLATE
1. Copy this file into pages/community/research/ and rename it to
   <short-slug>.md — e.g. <surname>_<institution>.md. The basename becomes the page
   URL, so keep it unique across the whole site and never leave it as the template name.
2. Fill in the front matter. Only title, type, description, researcher and affiliation
   are required. Delete any optional field you are not using rather than leaving the
   <placeholder> in place — an unfilled placeholder will render on the live page.
3. Write the project summary, then delete this comment block.

WHAT FEEDS THE STATISTICS BLOCK
  researcher   -> "Researchers supported" (counted once per unique name)
  affiliation  -> "Institutions" (counted once per unique name; keep institution names
                  spelled identically across pages or they will be counted twice)

DO NOT RECORD ALLOCATION DETAIL HERE
  This repository is public, and everything in this front matter is published with it.
  Per-project VM size, flavour, storage and start dates are kept out of the guide on
  purpose — record them privately rather than adding fields for them here.

The tile and the statistics block on the community page are generated automatically from
any page with type: "BioShell Research" — no need to edit community.md itself.
-->
