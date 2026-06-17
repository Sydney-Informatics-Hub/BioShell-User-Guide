---
title: Choosing a flavour
description: How to select the right combination of vCPUs and memory for your BioShell environment.
---

A flavour is the combination of virtual CPUs (vCPUs) and memory (RAM) allocated to your
BioShell environment. BioShell is a shared national resource, so you are encouraged to
request a flavour that closely matches your actual needs — this keeps capacity available for
everyone.

Estimating your requirements can be hard at the start of a project. Use the guidance below
to make a reasonable starting choice. You can always request a larger flavour later if your
analysis grows.

---

## Flavour types at a glance {#flavour-types}

| Type | Characteristics | Best for |
|------|----------------|----------|
| **t3** | Low memory relative to CPUs | Testing, small jobs, getting started |
| **m3** | Balanced CPU and memory | Most general-purpose workloads |
| **c3** | CPU-optimised (same memory ratio as m3) | Compute-heavy tasks such as alignment and assembly |
| **r3** | High memory per CPU | Memory-intensive analysis such as variant calling or large data processing |

### Available flavours

| vCPUs | RAM (GB) | Nectar flavours | Nirin flavours |
|-------|----------|----------------|----------------|
| 1 | 1 | t3.xsmall | c3.1c1m5d |
| 1 | 2 | m3.xsmall, c3.xsmall | c3.1c2m10d |
| 1 | 4 | r3.xsmall | — |
| 2 | 4 | m3.small, c3.small | c3.2c4m20d, c3.2c4m10d |
| 2 | 8 | r3.small | — |
| 4 | 8 | m3.medium, c3.medium | c3.4c8m20d, c3.4c8m10d |
| 4 | 16 | r3.medium | — |
| 8 | 16 | m3.large, c3.large | c3.8c16m20d, c3.8c16m10d |
| 8 | 32 | r3.large | — |
| 16 | 32 | m3.xlarge, c3.xlarge | — |
| 16 | 64 | r3.xlarge | — |
| 32 | 64 | m3.xxlarge, c3.xxlarge | — |
| 32 | 128 | r3.xxlarge | — |
| 64 | 128 | c3.3xlarge | — |

---

## Quick sizing guide {#sizing-guide}

| Task type | Suggested resources |
|-----------|-------------------|
| Light preprocessing (QC, trimming, filtering) | 1–4 vCPUs, 2–8 GB RAM |
| Alignment and assembly (e.g. `bwa`, `STAR`, `SPAdes`) | 8–16 vCPUs, 16–32 GB RAM |
| Memory-intensive analysis (variant calling, genome-wide statistics) | 16–32+ vCPUs, 32–128 GB RAM |
| Interactive analysis and visualisation (JupyterLab, RStudio) | 2–4 vCPUs, 8–16 GB RAM |

---

## Worked examples {#worked-examples}

### Light processing

John is starting a project analysing drought-resistant genes from 20 crop samples (~7 GB
raw sequencing data per sample). His pipeline includes quality control with `FastQC`,
adapter trimming, alignment to a reference genome, annotation, and phylogenetic tree
construction.

The most resource-intensive steps require moderate CPU and memory. Because John is analysing
a subset of genes rather than whole genomes, each run is relatively small:

- 2–4 vCPUs
- Up to 10 GB RAM

A balanced `m3` flavour is a good starting point:

- **Nectar:** `m3.medium` (4 vCPUs / 8 GB RAM)
- **Nirin:** `c3.4c8m10d` or `c3.4c8m20d` (4 vCPUs / 8 GB RAM)

### Memory-intensive processing

Georgie is running `GATK4` best-practice variant calling on 15 human exome samples. Each
sample produces BAM files of ~15 GB, with similar-sized temporary files during processing.
Total storage is approximately 1 TB. `GATK4` tools benefit from both high memory and
multiple CPU cores:

- 8 vCPUs
- 32 GB RAM

Recommended flavours for ~15 exomes:

- **Nectar:** `r3.large` (8 vCPUs / 32 GB RAM)
- **Nirin:** `c3.8c16m20d` or `c3.8c16m10d` (8 vCPUs / 16 GB RAM)

If processing ~30 exomes or running multiple jobs in parallel, consider `r3.xlarge`
(16 vCPUs / 64 GB RAM) on Nectar, or multiple 8 vCPU Nirin instances.

---

## How to choose if you are unsure {#how-to-choose}

1. **Check software documentation** — most bioinformatics tools publish minimum and
   recommended system requirements.
2. **Start small and scale up** — begin with a smaller flavour. If jobs run slowly with
   CPU usage consistently near 100%, you need more vCPUs. If jobs fail with memory errors,
   you need more RAM.
3. **Review previous runs** — if you have run similar analyses before, check your peak CPU
   and RAM usage from those logs to guide your estimate.

> **Note:** For interactive work in JupyterLab or RStudio, 2–4 vCPUs and 8–16 GB RAM is
> sufficient for most datasets. Increase memory if you are loading large files directly
> into your session.
