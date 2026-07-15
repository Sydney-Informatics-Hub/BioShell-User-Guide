---
title: Choosing the right environment size
type: Using BioShell
description: How to choose the right number of CPUs, memory, and storage for your BioShell environment.
---

{% include callout.html type="note" content="A flavour is the combination of virtual CPUs and memory allocated to your BioShell environment, essentially the “spec” of your cloud computer. Different flavours suit different workloads, just as you might choose a lightweight laptop for email but a workstation for video editing. You pick a flavour when you request access and can request a change later if your needs grow." %}

Cloud systems are shared research resources. As a general principle, you are encouraged to
request resources that closely match your actual needs. This supports fair access for all
users and preserves capacity for everyone.

Estimating requirements can be challenging, particularly at the start of a project you may
not yet know which software tools you will use or how demanding they will be. The guidance
below is designed to help you make a reasonable first choice and adjust from there.


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


## Suggested sizes by workload {#workload-sizes}

Each size below is also tagged with a **profile**, whether the workload leans on CPU,
memory, or both evenly. See [Understanding your workload profile](#workload-profile) at the
end of this page if you'd like more detail, or want to check your own usage.

### Light: preprocessing and exploration {#small}

**Who this suits:** users running quality control, adapter trimming, short read filtering, or
exploring tools and datasets interactively in JupyterLab or RStudio with small to moderate
datasets.

| | |
|---|---|
| **Profile** | Balanced |
| **CPUs** | 2–4 |
| **Memory** | 4–8 GB |
| **Storage** | Up to 100 GB |

**Example:** John is starting a research project analysing drought-resistant genes from 20
crop samples (~140 GB raw data). His pipeline runs quality control (`FASTQC`), adapter
trimming (`cutadapt`), alignment and annotation (`blast`, `SPAdes`), and phylogenetic tree
construction (`MrBayes`). Of these, `blast` and `SPAdes` are the most CPU- and
memory-intensive tools in the pipeline, but because John is selecting out a small set of
drought-resistant genes rather than whole genomes, each run only needs 2–4 CPUs and
under 10 GB of RAM. A balanced environment at this size handles the pipeline
comfortably. If John later extends the analysis to many more genes, those steps become more
CPU-bound and he should move to the medium size below with more cores.


### Moderate: single-cell RNA-seq analysis {#medium}

**Who this suits:** users running interactive multi-step R or Python analysis workflows,
particularly those involving large in-memory data objects.

| | |
|---|---|
| **Profile** | Memory-bound |
| **CPUs** | 4–8 |
| **Memory** | 32–64 GB |
| **Storage** | Variable, depends on sample count |

**Example:** Michael is running the
[**SIH scRNAvigator notebooks**](https://github.com/Sydney-Informatics-Hub/scrna-analysis) in
RStudio. The workflow covers quality control, doublet detection, dataset integration, cell
annotation, differential gene expression, and pathway enrichment analysis. Integration and
doublet detection steps load large data objects into memory simultaneously, making this
workflow **memory-bound rather than CPU-bound**, RAM matters more than core count.

The scRNAvigator documentation recommends at least 32 GB of memory for local use. A 32 GB
environment is suitable for small to moderate cohorts; larger datasets may require 64 GB or
more. If you are unsure, start at 32 GB and scale up if jobs fail or run very slowly.


### Heavy: whole exome variant calling and high-throughput workflows {#large}

**Who this suits:** users running GATK best-practice workflows, genome-wide analyses, large
alignments, or multiple samples in parallel.

| | |
|---|---|
| **Profile** | Balanced (CPU and memory both matter) |
| **CPUs** | 8–16 |
| **Memory** | 32–64 GB |
| **Storage** | Up to 1 TB |

**Example:** Georgie is running `GATK4` variant calling across 15 human exomes, processing
from `fastq` to `VCF` with quality control. Raw `fastq` files are 4–7 GB each (two per
sample), and the intermediate BAM files she keeps are about 15 GB per sample, with other
temporary intermediates consuming a similar amount of space. Total storage across 15 exomes
is approximately 870 GB, round up to 1 TB to allow room for workflow outputs and reruns.

While RAM usage in the GATK4 toolset can be limited with command-line flags, many of its
tools support multithreading, using more CPU cores in parallel reduces processing time even
where a tool doesn't let you set thread count explicitly. `GATK4` therefore benefits from
both high memory and multiple CPU cores, making a balanced large environment appropriate
here. For around 30 exomes, consider scaling towards the top of this range.

For larger cohorts, or when running multiple jobs in parallel, consider requesting a
proportionally larger environment, splitting the workload across multiple instances, or
moving to an HPC system, see [Beyond a single environment](#beyond-single-environment)
below.


## Not sure where to start? Start small. {#start-small}

If you are uncertain, the best approach is to begin with a smaller environment and scale up
based on what you observe:

- If jobs run slowly and CPU usage is consistently near 100%, you likely need more CPUs.
- If jobs fail with memory errors, or RAM usage is consistently near the limit, increase
  memory.
- If both are highly utilised, move to a larger balanced configuration.

Scaling incrementally avoids over-allocating shared resources and makes it easier to
identify bottlenecks.

<details markdown="1" id="workload-profile-details">
<summary>Understanding your workload profile: balanced, CPU-bound, or memory-bound</summary>

#### Is your workload balanced, CPU-bound, or memory-bound? {#workload-profile}

Beyond overall size, it helps to think about the *shape* of your workload, not just how much
CPU and memory it needs, but which one it needs more of relative to the other. BioShell can
offer environments tuned to each profile, so it's worth identifying yours before requesting
an environment:

- **Balanced** — CPU and memory usage rise and fall together. This describes most
  general-purpose and interactive work (notebooks, mixed pipelines). Roughly equal parts
  CPU and RAM, in the range of 1 CPU to every 2 GB of RAM, is a reasonable default.
- **CPU-bound** — CPU usage sits at or near 100% while memory has plenty of headroom. This
  is typical of alignment, assembly, and other steps that scale with thread count. These
  workloads benefit from more cores relative to memory.
- **Memory-bound** — RAM usage is consistently high while CPU is under-utilised. This is
  typical of workflows that load large objects into memory at once, dataset integration,
  large in-memory joins, genome indices. These workloads benefit from more RAM relative to
  CPU, sometimes 1 CPU to every 4 GB of RAM or more.

{% include callout.html type="tip" content="If you're not sure which profile fits, request a balanced environment to start, then monitor CPU and memory usage while your pipeline runs (see Checking CPU and memory usage below). If one resource is consistently the bottleneck, mention this when requesting a change so you can move to an environment shaped for that profile." %}

#### Checking CPU and memory usage {#checking-usage}

To find out whether you're actually CPU-bound, memory-bound, or well matched, watch your
environment while a job is running. A few simple command-line tools make this easy, no
scripting or special permissions needed, connect to your environment in a second terminal
so your pipeline keeps running while you watch.

{% include callout.html type="tip" content="Usage only tells you something while a job is actually running. Start your pipeline first, then open one of the tools below in a separate terminal window or tab." %}

**`htop` (recommended for beginners)**

`htop` gives a live, colour-coded view that updates automatically, the easiest way to see
what's happening at a glance:

1. Run `htop`.
2. The numbered bars at the top show load on each CPU core. If most cores are consistently
   full (and shown in red/orange) while your job runs, your workload is **CPU-bound**.
3. The `Mem` bar shows how much RAM is in use. If it climbs steadily and sits near the total
   available, your workload is **memory-bound**.
4. Press `q` to quit.

If `htop` isn't already installed, install it with `sudo apt install htop` (Ubuntu/Debian
environments) or ask Bio-Shelley for help.

**`top`**

`top` shows the same core information as `htop` in a plainer, text-only layout, and is
installed by default on almost every Linux system, useful if `htop` isn't available:

- Run `top`.
- Press `1` to show usage for each CPU core individually rather than a single average.
- Press `q` to quit.

</details>


## Beyond a single environment: when to consider HPC {#beyond-single-environment}

BioShell environments are well suited to interactive work and moderate-scale pipelines, but
they have a ceiling. If your workload keeps growing, more samples in parallel, whole genomes
rather than subsets, cohorts scaling into the hundreds, a single environment may no longer
be the most efficient option.


{% include callout.html type="tip" content="Before requesting a very large environment, test your pipeline end-to-end on a small subset of your data (a handful of samples, or a reduced reference) on a modest environment. This confirms the pipeline runs correctly and gives you a realistic estimate of per-sample time and resource use, information you’ll need whether you stay on BioShell or move to an HPC system." %}


Once your pipeline is validated, high-throughput or many-sample workloads are often better
suited to a national HPC facility than to a single cloud environment. The
[**Australian BioCommons Leadership Share (ABLeS)**](https://australianbiocommons.github.io/ables/index)
programme, offering access to HPC infrastructure, specialist expertise, and best-practice
support, can help you plan that transition.
