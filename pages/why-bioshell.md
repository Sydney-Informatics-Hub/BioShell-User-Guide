---
title: Why use a cloud workspace for bioinformatics?
description: An introduction to virtual machines, cloud computing, and why BioShell is a great starting point for life-science researchers and workshop participants.
---

If you are new to bioinformatics or planning a workshop, this page explains what cloud
computing is, when to use it, and why BioShell makes it easy to get started.

---

## What is a virtual machine? {#what-is-a-vm}

A virtual machine is a computer that runs inside another computer — delivered to you over the
internet. You connect to it from your laptop or desktop, and it behaves like a full computer
you have complete control over. You do not need to worry about your own hardware specs,
operating system, or software installation.

> Think of it like a fully-equipped lab bench that is always ready — you just sit down and
> start working, then leave when you are done. The bench does not belong to you, but it is
> set up exactly the way you need it.

With a virtual machine you can:

- Get more computing power than your laptop for data analysis
- Share the same environment with colleagues or workshop participants
- Run long jobs overnight without worrying about power outages or closing your laptop
- Use a Linux environment even if you normally use Windows or macOS
- Have administrator access to install software and configure the system
- Host services such as databases or web servers

---

## Cloud vs HPC — which should I use? {#cloud-vs-hpc}

Both cloud computing and High-Performance Computing (HPC) systems such as NCI Gadi are
powerful research tools, but they suit different types of work.

| | Cloud (BioShell) | HPC (e.g. NCI Gadi) |
|--|-----------------|---------------------|
| **Session type** | Interactive — work in real time | Batch — submit jobs and come back later |
| **Interfaces** | JupyterLab, RStudio, CLI | CLI only (PBS/Slurm schedulers) |
| **Setup required** | None — environment is preconfigured | Familiarity with job schedulers required |
| **Best for** | Learning, workshops, exploratory analysis | Large-scale, tightly coupled, or long-running workloads |
| **Scaling** | Flexible — choose your VM size on demand | Fixed allocations managed through project quotas |

**Not sure which to use?** If you are learning bioinformatics, running a workshop, or
exploring a new analysis, start with cloud. BioShell is the right choice. You can move to
HPC later once your workflow is established and you need more compute power.

---

## Why BioShell? {#why-bioshell}

Getting started on the command line is often the hardest part. Software installs break.
Environments differ between computers. Trainers spend hours configuring machines instead of
teaching. BioShell solves this.

| Feature | What it means for you |
|---------|----------------------|
| **Ready to use** | Over 20,000 bioinformatics tools and reference datasets available — no setup required |
| **Consistent everywhere** | The same environment works identically on Nectar and NCI Nirin |
| **Training-ready** | Trainers can provision dozens of identical machines for a workshop in minutes |
| **Multiple interfaces** | Access via SSH, or use JupyterLab and RStudio in your browser |
| **Reproducible** | Containerised software means your analysis works the same way today, next year, and on a colleague's machine |
| **Safe to explore** | You have administrator access to your own VM — mistakes only affect your own instance |

---

## How does BioShell fit with HPC? {#bioshell-and-hpc}

BioShell is not a replacement for large-scale HPC — it complements it. Think of it as the
place where you learn, develop, and test your workflows before scaling up.

1. **Learn and explore on BioShell** — get comfortable with the command line, tools, and
   your data in a low-pressure environment.
2. **Develop and test your workflow** — build and validate your Nextflow or Snakemake
   pipeline using BioShell's pre-installed tools and example datasets.
3. **Scale up to HPC when ready** — once your workflow is confirmed and your data needs are
   clear, move to NCI Gadi or another HPC system for large-scale runs.

---

## Ready to get started? {#get-started}

BioShell is available to Australian life-science researchers, trainers, and developers
through [ABLeS](https://australianbiocommons.github.io/ables/). Access is free — you just
need to be affiliated with an Australian research organisation.

[Check your eligibility and apply →](access#eligibility)
