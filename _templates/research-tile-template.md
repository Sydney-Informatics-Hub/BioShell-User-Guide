---
title: "<Project name>"
type: "BioShell Research"
description: "<1–2 sentence project summary shown on the community tile>"
researcher: "<Researcher name>"   # counted in the statistics block
affiliation: "<Institution>"      # counted as an institution in the statistics block
# Several people on this one project? Delete the `researcher:` line above, uncomment the
# block below, and list everyone. `affiliation:` stays as the default institution for
# anyone without an affiliation of their own.
# researchers:
#   - name: "<Researcher name>"
#   - name: "<Researcher name>"
#     affiliation: "<Their institution, only if different>"
collaborators: "<names>"          # optional
funding: "<funders>"              # optional
doi: <10.xxxx/xxxxx>              # optional — publication or dataset DOI, without the https://doi.org/ prefix
---

{% include research-people.html %}

**Project:** {{ page.title }}

<1–2 sentence summary of the research project and how BioShell was used.>

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
2. Fill in the front matter. Only title, type, description, and either researcher or
   researchers plus affiliation are required. Delete any optional field you are not using
   rather than leaving the <placeholder> in place — an unfilled placeholder will render on
   the live page.
3. Write the project summary, then delete this comment block.

ONE PERSON OR SEVERAL
  One person   researcher: "Dr A"  +  affiliation: "Institution"
  Several      researchers:
                 - name: "Dr A"                        # uses the page-level affiliation
                 - name: "Dr B"
                   affiliation: "Another institution"  # only when it differs
  Use one shape or the other. `researchers` wins if both are present, so delete
  `researcher:` when you convert a page — otherwise that person is credited nowhere.
  Everyone listed is named on the community tile and counted in the statistics.

  `collaborators` is a different thing: free text for people and groups who contributed
  to the science but are not BioShell users. It is not counted in the statistics.

WHAT FEEDS THE STATISTICS BLOCK
  researcher / researchers[].name -> "Researchers supported" (counted once per unique name)
  affiliation / researchers[].affiliation -> "Institutions" (counted once per unique name;
                  keep institution names spelled identically across pages or they will be
                  counted twice)

DO NOT RECORD ALLOCATION DETAIL HERE
  This repository is public, and everything in this front matter is published with it.
  Per-project VM size, flavour, storage and start dates are kept out of the guide on
  purpose — record them privately rather than adding fields for them here.

The tile and the statistics block on the community page are generated automatically from
any page with type: "BioShell Research" — no need to edit community.md itself.
-->
