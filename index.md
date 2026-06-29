---
title: BioShell
description: A guide to accessing and using BioShell, a preconfigured cloud bioinformatics
  environment available on NCI Nirin and ARDC Nectar Research Cloud.
sidebar: false
---

<p style="text-align:center; margin: 1rem 0;"><img src="{{ '/images/BioShell_logo_with_name.png' | relative_url }}" alt="BioShell logo" style="max-width:300px;"></p>

## Introduction

BioShell is a ready-to-use cloud bioinformatics environment designed to get you from data to
results with as little setup as possible. Rather than spending time configuring software on
your own machine, you connect to BioShell and find a fully equipped workspace already waiting
for you — with the tools, compute power, and interactive environments you need to run analyses
that would be impractical on a laptop. 

Whether you are a higher degree student running your
first bioinformatics analysis or a trainer delivering a national workshop, BioShell removes
the technical overhead so you can focus on delivering science.

This guide walks you through requesting access, choosing the right environment for your work,
and using BioShell's built-in tools and support features. It assumes you are comfortable
with basic command-line navigation, but no prior experience with cloud infrastructure,
containers, or software installation is needed. BioShell is developed by Australian BioCommons
as part of the [ABLeS programme](https://australianbiocommons.github.io/ables/) and supports
the BioCommons Training cooperative and
[Bioinformatics ToolFinder](https://australianbiocommons.github.io/2_tools.html).

> **Note:** This guide does not cover how to provision a virtual machine on Nectar or NCI, or
> how to build a BioShell instance from scratch. See the
> [BioShell GitHub repository](https://github.com/AustralianBioCommons/BioShell) and the
> infrastructure documentation for those topics.

---

## Quick start guide

> **Tip:** New to cloud computing or virtual machines? Read [Why use a cloud workspace?](why-bioshell)
> first — it explains what BioShell is, how it compares to HPC, and whether it is right for your work.

1. [Check you are eligible](access#eligibility) — confirm your project is life-science
   related and aligns with the five ABLeS principles.
2. [Identify your access pathway](access#pathways) — most users apply through
   [ABLeS](https://australianbiocommons.github.io/ables/eligibility); training organisers
   contact the [BioCommons Training Team](https://www.biocommons.org.au/event-support).
3. [Submit your access request](access#requesting-access) — submit via the appropriate
   pathway and wait for your infrastructure placement to be confirmed.
4. [Accept your terms and conditions](access#terms) — accept the terms for your assigned
   infrastructure before access is granted.
5. [Connect to your BioShell environment via SSH](access#connecting) — use the connection
   details provided in your provisioning notification.
6. [Choose your flavour](using-bioshell#flavours) — select the preconfigured environment
   profile that matches your task.
7. [Load a tool and run your analysis](using-bioshell#tools) — use Bio-Shelley or
   `module load` to access tools from the BioShell tool library.
8. [Launch an interactive environment](using-bioshell#interactive) — open JupyterLab or
   RStudio directly in your browser for notebook-based work.

> **Tip:** Not sure which tools are available or how to load them? Run
> `shelley-bio search "<what you want to do>"` for instant suggestions.

---

## How to cite

If you use BioShell in your research or training, please cite it as:

> O'Brien M, Jaya F, Samaha G, Xue W, Al Bkhetan Z, Botting A, Gustafsson J. (2026).
> **BioShell how-to guide**. Australian BioCommons.
> [AUTHOR TO SUPPLY — Zenodo DOI once minted]

---

![](images/bioshell/SCREENSHOT_NEEDED_overview_diagram.png)

*Fig 1. Overview of the BioShell access and use flow: from submitting an access request
through to connecting, choosing a flavour, loading tools, and launching interactive
environments.*

---

## Operational partners

BioShell is delivered through a collaboration between Australian BioCommons, the Sydney
Informatics Hub at the University of Sydney, ARDC Nectar Research Cloud, and the National
Computational Infrastructure.

<div style="display: flex; flex-wrap: wrap; gap: 2rem; align-items: center; margin: 1.5rem 0;">
  <img src="{{ '/assets/img/Australian-Biocommons-Logo-Horizontal-RGB.png' | relative_url }}" alt="Australian BioCommons" style="max-height: 48px;">
  <img src="{{ '/assets/img/University%20of%20Sydney%20logo%20lockup%20-%20Sydney%20Informatics%20Hub%20-%20mono.png' | relative_url }}" alt="Sydney Informatics Hub, University of Sydney" style="max-height: 48px;">
  <img src="{{ '/assets/img/ARDC_Nectar_Research_Cloud_RGB.png' | relative_url }}" alt="ARDC Nectar Research Cloud" style="max-height: 48px;">
  <img src="{{ '/assets/img/NCI%20logo.png' | relative_url }}" alt="National Computational Infrastructure" style="max-height: 48px;">
</div>
