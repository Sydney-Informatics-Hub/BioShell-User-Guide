---
title: Choosing the right environment size
type: Getting started
description: How to choose the right number of CPUs, memory, and storage for your BioShell environment.
---

> **What is a flavour?**
> A flavour is the combination of virtual CPUs and memory allocated to your BioShell
> environment, essentially the "spec" of your cloud computer. Different flavours suit
> different workloads, just as you might choose a lightweight laptop for email but a
> workstation for video editing. You pick a flavour when you request access and can request
> a change later if your needs grow.

Cloud systems are shared research resources. As a general principle, you are encouraged to
request resources that closely match your actual needs. This supports fair access for all
users and preserves capacity for everyone.

Estimating requirements can be challenging, particularly at the start of a project you may
not yet know which software tools you will use or how demanding they will be. The guidance
below is designed to help you make a reasonable first choice and adjust from there.

---

## A familiar starting point {#familiar-starting-point}

A good way to think about environment sizing is to start from what you already know: your
laptop.

A standard modern laptop typically has 4–8 CPU cores and 8–16 GB of memory. A BioShell
environment of equivalent size will run anything your laptop can handle, and often faster,
because the environment does not share resources with a browser, email client, or other
background applications.

If you are new to BioShell, or unsure of your requirements, starting with a
**laptop-equivalent size** (4 CPUs / 8–16 GB RAM) is a reasonable default. You can always
request a larger environment if you find you need it.

---

## Suggested sizes by workload {#workload-sizes}

### Small: light preprocessing and exploration {#small}

**Who this suits:** users running quality control, adapter trimming, short read filtering, or
exploring tools and datasets interactively in JupyterLab or RStudio with small to moderate
datasets.

| | |
|---|---|
| **CPUs** | 2–4 |
| **Memory** | 4–8 GB |
| **Storage** | Up to 100 GB |

**Example:** John is starting a research project analysing drought-resistant genes from 20
crop samples (~140 GB raw data). His pipeline covers quality control, trimming, alignment,
annotation, and phylogenetic tree construction. Because he is working on a subset of genes
rather than whole genomes, each individual job is small. A standard laptop-equivalent
environment handles this comfortably.

---

### Medium: single-cell RNA-seq analysis {#medium}

**Who this suits:** users running interactive multi-step R or Python analysis workflows,
particularly those involving large in-memory data objects.

| | |
|---|---|
| **CPUs** | 4–8 |
| **Memory** | 32–64 GB |
| **Storage** | Variable, depends on sample count |

**Example:** A researcher running the
[SIH scRNAvigator notebooks](https://github.com/Sydney-Informatics-Hub/scrna-analysis) in
RStudio. The workflow covers quality control, doublet detection, dataset integration, cell
annotation, differential gene expression, and pathway enrichment analysis. Integration and
doublet detection steps load large data objects into memory simultaneously, making this
workflow **memory-bound rather than CPU-bound**, RAM matters more than core count.

The scRNAvigator documentation recommends at least 32 GB of memory for local use. A 32 GB
environment is suitable for small to moderate cohorts; larger datasets may require 64 GB or
more. If you are unsure, start at 32 GB and scale up if jobs fail or run very slowly.

---

### Large: whole exome variant calling and high-throughput workflows {#large}

**Who this suits:** users running GATK best-practice workflows, genome-wide analyses, large
alignments, or multiple samples in parallel.

| | |
|---|---|
| **CPUs** | 8–16 |
| **Memory** | 32–64 GB |
| **Storage** | Up to 1 TB |

**Example:** Georgie is running `GATK4` variant calling across 15 human exomes. Each sample
produces approximately 15 GB of BAM files, plus similarly sized temporary intermediates.
Total storage for the project is approximately 870 GB, round up to 1 TB to allow room for
workflow outputs and reruns. `GATK4` benefits from both high memory and multiple CPU cores,
so a balanced large environment is appropriate here.

For larger cohorts (~30 exomes or more), or when running multiple jobs in parallel, consider
requesting a proportionally larger environment or splitting the workload across multiple
instances.

---

## Not sure where to start? Start small. {#start-small}

If you are uncertain, the best approach is to begin with a smaller environment and scale up
based on what you observe:

- If jobs run slowly and CPU usage is consistently near 100%, you likely need more CPUs.
- If jobs fail with memory errors, or RAM usage is consistently near the limit, increase
  memory.
- If both are highly utilised, move to a larger balanced configuration.

Scaling incrementally avoids over-allocating shared resources and makes it easier to
identify bottlenecks.

---

## Quick reference {#quick-reference}

| Workload | CPUs | Memory | Storage |
|----------|------|--------|---------|
| Light — QC, trimming, small datasets | 2–4 | 4–8 GB | Up to 100 GB |
| Moderate — alignment, assembly, interactive notebooks | 4–8 | 8–32 GB | 100–500 GB |
| Heavy — variant calling, large-scale analysis, multi-sample workflows | 8–16 | 32–64 GB | 500 GB – 1 TB |
| Very large — genome-wide, high memory workflows | 16–32 | 64–128 GB | 1 TB+ |

---

## Further reading {#further-reading}

For the full list of available environment sizes on each platform, see:

- [Nectar flavours](https://support.ehelp.edu.au/support/solutions/articles/6000205341-nectar-flavors)
- [Nirin flavours and charge rates](https://opus.nci.org.au/spaces/Help/pages/152207479/Nirin+Flavors+and+Charge+Rates)
