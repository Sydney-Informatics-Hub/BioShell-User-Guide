---
title: Introduction to the Linux Command Line Workshop
type: "BioShell Training"
description: "Allan J, Merce C, Kadolsky U, Zhang A (2026)"  
date: 2026
participants: 18 
materials_url: https://datacarpentry.github.io/shell-genomics/aio.html
---



This workshop aims to provide an introduction to command-line Linux targeted at researchers. It will take them from basic commands through to running an introductory command line tool.

This workshop was run by Genomics WA

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
