---
title: Why use a cloud workspace for bioinformatics?
type: Getting started
description: An introduction to virtual machines, cloud computing, and why BioShell is a great starting point for life-science researchers and workshop participants.
---

New to bioinformatics or high-performance computing? This page explains what a cloud workspace is, when it's the right tool, and why BioShell is an easy place to start.

{% include callout.html type="note" content="Why 'cloud'? The name comes from old network diagrams, where engineers would draw a cloud symbol for the internet or any infrastructure whose exact physical location didn't matter to the diagram. The name stuck. Your work really does run on physical computers in a data centre somewhere, but you never need to know where. You just connect over the internet and use them." %}


## What is a virtual machine? {#what-is-a-vm}

A virtual machine (or VM) is a computer hosted in a data centre that you access over the internet. You connect from your own laptop, and it behaves like a normal computer, just far more powerful than your own.

It's your own private space. You can set it up however you like and install any software you need without asking for permission or waiting for administrator help. If something goes wrong, it only affects your own machine.

{% include callout.html type="tip" content="Think of it like having your own research lab. You don't need to worry about installing the lights or plumbing. That's all already there and working. You walk in, set up your workspace the way you like it, and get to work on what actually matters: your research. If you need a new tool or more space, you just ask and it's there. Best of all, you have the freedom to experiment and learn without worrying about breaking something someone else depends on. It's your space to make your own." %}

A VM like this lets you:

- Use far more computing power than your laptop can offer
- Keep long analyses running even after you close your laptop
- Work in a Linux environment, the operating system most bioinformatics tools are built for, even if you normally use Windows or macOS
- Share the same environment with colleagues or workshop participants
- Have administrator access to install software and configure the system

## Why BioShell? {#why-bioshell}

Bioinformatics analyses typically require many specialised tools chained together, each with its own dependencies. Version conflicts are common because one tool might need an older version of a library while another needs a newer one. This makes setting up all these tools time-consuming, and once you do, your analysis is difficult to reproduce on a different machine.

BioShell solves this by using open standards. All tools come from BioContainers (the same public repository that Galaxy uses), retrieved and installed by Shelley. Because BioShell itself is version-controlled and built on public infrastructure, your analysis stays reproducible and portable. You skip the installation overhead and get a research environment that travels with you.

| | What it means for you |
|--|----------------------|
| **Ready to use** | Every user gets the same pre-configured environment. No setup needed before you start |
| **Built on open standards** | You get access to tools that are already built, vetted, and documented by the bioinformatics community |
| **Find and install tools easily** | [Shelley](tools) handles the work of retrieving containers, installing them, and showing you what each tool does. Install any of 20,000+ bioinformatics tools with a single command: `shelley build <tool>` |
| **Truly reproducible** | Because BioShell is version-controlled and BioContainers are public, you can rebuild your entire analysis identically on any machine, share it with colleagues, or return to it years later. No more environment inconsistency between laptops, HPCs, or VMs |


## HPC: an alternative for large-scale work {#cloud-vs-hpc}

You may also have heard of HPC, short for High-Performance Computing. An HPC system is a large, shared computer, such as the NCI Gadi supercomputer, that many researchers use simultaneously. While both cloud workspaces and HPC systems are powerful tools, they're suited to different kinds of work.

The main difference is how you use them. On cloud machines like BioShell, you work interactively, the way you would on your own computer. On HPC, you write out the steps of your analysis like a recipe, submit it to a shared queue, and wait for the results. HPC is efficient for very large jobs, but it has a steeper learning curve.

| | BioShell (cloud) | HPC (e.g. NCI Gadi) |
|--|-----------------|---------------------|
| **How you work** | Interactively, in real time | Submit jobs to a queue and retrieve results later |
| **How you connect** | Web tools (JupyterLab, RStudio) or the command line | Command line only |
| **Getting set up** | Self-service and quick; you install your own tools | Requires an approved allocation and administrator setup; knowledge of job queues needed |
| **Best for** | Learning, workshops, and exploring new analyses | Large-scale or long-running analyses that benefit from parallel processing |

## When to use BioShell {#when-to-use}

If you are learning bioinformatics, running a workshop, or exploring a new analysis, BioShell is a good choice:

1. **Learn and explore on BioShell.** Get comfortable with the command line, your tools, and your data.
2. **Build and test your analysis.** Put together your analysis pipeline (for example, one written in Nextflow) and check that it works using BioShell's ready-to-go tools and example data.
3. **Move to HPC if needed.** If your analysis requires significant computing power or will run for extended periods, a HPC system like NCI Gadi is better suited to those demands.

## [Ready to get started? Check your eligibility and apply →](access#eligibility) {#get-started} 
