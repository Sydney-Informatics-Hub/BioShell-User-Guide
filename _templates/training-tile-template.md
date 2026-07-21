---
title: "<Workshop title>"
type: "BioShell Training"
description: "<Authors> (<Year>)"   # short byline shown on the community tile
date: <YYYY>                         # year the event ran (used for the statistics block)
participants: <number>               # optional — people trained (counted in the statistics block)
doi: <10.5281/zenodo.xxxxxxxx>       # optional — Zenodo (or other) DOI, without the https://doi.org/ prefix
materials_url:                       # optional — link to materials if not the DOI above
---

<1–2 sentence description of the workshop: what it covered and who it was for.>

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
