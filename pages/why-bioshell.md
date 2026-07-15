---
title: Why use a cloud workspace for bioinformatics?
type: Getting started
description: An introduction to virtual machines, cloud computing, and why BioShell is a great starting point for life-science researchers and workshop participants.
---

If you are new to bioinformatics and the world of high computing, this page explains what cloud
computing is, when to use it, and why BioShell makes it easy to get started.

{% include callout.html type="note" content="Why 'cloud'? The name comes from old network diagrams, engineers would draw a cloud symbol for the internet or any infrastructure whose exact physical location did not matter to the diagram. The name stuck: your data and computing actually run on real servers in a data centre somewhere, but you do not need to know or care where, you just connect over the internet and use it." %}


## What is a virtual machine? {#what-is-a-vm}

A virtual machine is a computer that runs inside another computer, delivered to you over the
internet. You connect to it from your laptop or desktop, and it behaves like a full computer
you have complete control over. You do not need to worry about your own hardware specs or
operating system, and unlike an HPC system, you have the administrator access to install and
configure software yourself, no need to work around module systems or wait on sysadmin
approval. [Shelley](tools) is there to make that installation step fast.

{% include callout.html type="tip" content="Think of it like a fully-equipped lab bench that is always ready, you just sit down and start working, then leave when you are done. The bench does not belong to you, but it is set up exactly the way you need it." %}

With a virtual machine you can:

- Get more computing power than your laptop for data analysis
- Share the same environment with colleagues or workshop participants
- Run long jobs overnight without worrying about power outages or closing your laptop
- Use a Linux environment even if you normally use Windows or macOS
- Have administrator access to install software and configure the system
- Host services such as databases or web servers


## Cloud vs HPC - which should I use? {#cloud-vs-hpc}

Both cloud computing and High-Performance Computing (HPC) systems such as NCI Gadi are
powerful research tools, but they suit different types of work.

| | Cloud (BioShell) | HPC (e.g. NCI Gadi) |
|--|-----------------|---------------------|
| **Session type** | Interactive - work in real time | Batch - submit jobs and come back later |
| **Interfaces** | JupyterLab, RStudio, CLI | CLI only (PBS/Slurm schedulers) |
| **Setup required** | You install your own tools (Shelley makes this quick), with full admin control and none of an HPC's module or approval restrictions | Familiarity with job schedulers required |
| **Best for** | Learning, workshops, exploratory analysis | Large-scale, tightly coupled, or long-running workloads |
| **Scaling** | Flexible - choose your VM size on demand | Fixed allocations managed through project quotas |

**Not sure which to use?** If you are learning bioinformatics, running a workshop, or
exploring a new analysis, start with cloud. BioShell is the right choice. You can move to
HPC later once your workflow is established and you need more compute power.


## Why BioShell? {#why-bioshell}

Getting started on the command line is often the hardest part. Software installs break.
Environments differ between computers. Trainers spend hours configuring machines instead of
teaching. BioShell solves this.

| Feature | What it means for you |
|---------|----------------------|
| **Ready on any device** | Every participant gets the same preconfigured environment regardless of whether they are on Windows, macOS, or Linux with no device-specific setup required |
| **More power than your laptop** | Use familiar tools like RStudio and JupyterLab, but backed by cloud compute that is not competing with your browser, email, or background applications |
| **Tool discovery built in** | [Shelley](tools), BioShell's command-line assistant, makes finding and installing tools from a catalogue of over 20,000 containers as simple as `Shelley build samtools`, no container knowledge needed |
| **Reproducible workshops** | Version-controlled environments mean you can build a workshop once and rerun it identically next year, or share it with another trainer at another institution |
| **Easier access than HPC** | Getting time on a national HPC requires project allocation, queue estimates, and job scheduler knowledge. BioShell access is fast, self-service, and does not require you to know in advance exactly what you will run |
| **Admin access, safely sandboxed** | You have full administrator privileges on your own VM, install anything and break things without affecting other users or needing HPC sysadmin approval |


## How does BioShell fit with HPC? {#bioshell-and-hpc}

BioShell is not a replacement for large-scale HPC, it complements it. Think of it as the
place where you learn, develop, and test your workflows before scaling up. It helps fill the gap between working on your laptop and moving onto a HPC

1. **Learn and explore on BioShell** - get comfortable with the command line, tools, and
   your data in a low-pressure environment.
2. **Develop and test your workflow** - build and validate your Nextflow or Snakemake
   pipeline using BioShell's pre-installed tools and example datasets.
3. **Scale up to HPC when ready** - once your workflow is confirmed and your data needs are
   clear, move to NCI Gadi or another HPC system for large-scale runs.


## [Ready to get started? Check your eligibility and apply →](access#eligibility) {#get-started} 
