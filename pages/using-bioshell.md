---
title: Using BioShell
description: How to choose a flavour, load tools, use interactive environments, and get
  help from Bio-Shelley in BioShell.
---

## Overview

Once you are connected to your BioShell environment, you have access to a curated library
of bioinformatics tools, interactive coding environments, and an AI-assisted support agent.
This page walks you through each feature.

---

## Choosing a flavour {#flavours}

A flavour is the combination of virtual CPUs (vCPUs) and memory (RAM) allocated to your
BioShell environment. BioShell is a shared national resource, so you are encouraged to
request a flavour that closely matches your actual needs — this keeps capacity available for
everyone.

Estimating your requirements can be hard at the start of a project. Use the guidance below
to make a reasonable starting choice. You can always request a larger flavour later if your
analysis grows.

### Flavour types at a glance

| Type | Characteristics | Best for |
|------|----------------|----------|
| **t3** | Low memory relative to CPUs | Testing, small jobs, getting started |
| **m3** | Balanced CPU and memory | Most general-purpose workloads |
| **c3** | CPU-optimised (same memory ratio as m3) | Compute-heavy tasks such as alignment and assembly |
| **r3** | High memory per CPU | Memory-intensive analysis such as variant calling or large data processing |

### Quick sizing guide

| Task type | Suggested resources |
|-----------|-------------------|
| Light preprocessing (QC, trimming, filtering) | 1–4 vCPUs, 2–8 GB RAM |
| Alignment and assembly (e.g. `bwa`, `STAR`, `SPAdes`) | 8–16 vCPUs, 16–32 GB RAM |
| Memory-intensive analysis (variant calling, genome-wide statistics) | 16–32+ vCPUs, 32–128 GB RAM |
| Interactive analysis and visualisation (JupyterLab, RStudio) | 2–4 vCPUs, 8–16 GB RAM |

### Worked examples

#### Light processing

John is starting a project analysing drought-resistant genes from 20 crop samples (~7 GB
raw sequencing data per sample). His pipeline includes quality control with `FastQC`,
adapter trimming, alignment to a reference genome, annotation, and phylogenetic tree
construction.

The most resource-intensive steps require moderate CPU and memory. Because John is analysing
a subset of genes rather than whole genomes, each run is relatively small:

- 2–4 vCPUs
- Up to 10 GB RAM

A balanced `m3` flavour (4 vCPUs / 8 GB RAM) is a good starting point.

#### Memory-intensive processing

Georgie is running `GATK4` best-practice variant calling on 15 human exome samples. Each
sample produces BAM files of ~15 GB, with similar-sized temporary files during processing.
Total storage is approximately 1 TB. `GATK4` tools benefit from both high memory and
multiple CPU cores:

- 8 vCPUs
- 32 GB RAM

An `r3` equivalent (8 vCPUs / 32 GB RAM) is suitable. If processing ~30 exomes or running
multiple jobs in parallel, consider doubling the CPU and memory allocation.

### How to choose if you are unsure

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

---

## Reproducible tools {#tools}

BioShell gives you access to thousands of bioinformatics tools through
[CernVM-FS (CVMFS)](https://cvmfs.readthedocs.io/en/stable/), a read-only shared file system
that delivers containerised tools and reference datasets on demand. Rather than downloading
or installing software yourself, you load tools as modules — they are pulled from CVMFS only
when you need them.

Through CVMFS, BioShell connects to three repositories:

| Repository | Contents |
|-----------|----------|
| `singularity.galaxyproject.org` | Containerised tools from [BioContainers](https://biocontainers.pro/) |
| `data.galaxyproject.org` | Reference genome builds and pre-built indexes from the Galaxy Project |
| `data.biocommons.aarnet.edu.au` | [AUTHOR TO SUPPLY — confirm final name and description] |

> **Note:** CVMFS does not download everything in advance. It fetches files only when you
> access them, so the first time you use a tool or dataset it may take a moment to load.
> Subsequent access is faster because the files are cached locally.

### Step 1 — Verify your CVMFS connection

When you first connect to BioShell you will see the following welcome message:

```
##################################################################
#                                                                #
# Welcome to BioShell!                                           #
#                                                                #
# A range of commonly used bioinformatics software is available  #
# via CVMFS.                                                     #
#                                                                #
# To make the software folders visible, run:                     #
#   cvmfs_config probe                                           #
#                                                                #
# If you need help finding and installing software,              #
# ask Shelley-Bio                                                #
#                      _   .----.                                #
#                     (_ \/      \_,                             #
#                        `uu----uu'                              #
# Basic commands:                                                #
# shelley-bio find <tool>                                        #
# shelley-bio search "<function>"                                #
# shelley-bio versions <tool>                                    #
# shelley-bio build <tool>                                       #
# shelley-bio interactive                                        #
##################################################################
```

Run the probe command to confirm CVMFS is connected:

```bash
cvmfs_config probe
```

You should see `OK` for each repository. If a repository shows `Failed!`, wait a moment and
try again. Contact [Australian BioCommons support](https://www.biocommons.org.au/helpdesk)
if the problem persists.

### Step 2 — Find and install tools with Bio-Shelley {#bio-shelley}

**Bio-Shelley** is BioShell's built-in assistant for finding and installing tools. It indexes
over 700 tools and 118,000 container versions from the BioContainers catalogue so you do not
need to know container paths or SHPC commands.

You can use Bio-Shelley in two ways:

**Directly from the command line:**

```bash
shelley-bio find <tool>
shelley-bio search "<function>"
shelley-bio versions <tool>
shelley-bio build <tool>
```

**Or in interactive mode:**

```bash
shelley-bio interactive
```

| Command | What it does | Example |
|---------|-------------|---------|
| `find <tool>` | Look up a specific tool by name | `shelley-bio find fastqc` |
| `search "<function>"` | Search by keyword or function | `shelley-bio search "quality control"` |
| `versions <tool>` | List all available versions | `shelley-bio versions samtools` |
| `build <tool[/version]>` | Install the tool as a loadable module | `shelley-bio build samtools/1.21` |
| `interactive` | Launch Bio-Shelley in interactive mode | `shelley-bio interactive` |

The `build` command handles the full installation automatically — it finds the correct
container in CVMFS, generates the module file, and makes the tool ready to load.

![](images/bioshell/SCREENSHOT_NEEDED_bioshelley_build.png)

*Fig 3. Using `shelley-bio build samtools/1.21` to install a tool module.*

> **Tip:** Use `shelley-bio search` when you know what you want to do but not the tool name.
> For example, `shelley-bio search "variant calling"` returns tools relevant to that task.

### Step 3 — Load a tool

Once Bio-Shelley has built a module, load it with:

```bash
module use ~/shpc/modules
module load <tool>/<version>
```

Verify the tool is ready:

```bash
<tool> --version
```

> **Note:** Run `module use ~/shpc/modules` each session, or add it to your `~/.bashrc`
> to make it permanent.

---

## Reference datasets {#reference-data}

CVMFS also provides access to reference genome builds and pre-built indexes from the Galaxy
Project. You can use these directly in your analyses by referencing their absolute file path.

List the available reference data:

```bash
ls /cvmfs/data.galaxyproject.org/byhand/
ls /cvmfs/data.galaxyproject.org/managed/
```

| Directory | Contents |
|-----------|---------|
| `/managed` | Datasets generated with Galaxy Data Manager tools. Organised by index type, then genome build. |
| `/byhand` | Older, manually curated datasets. Organised by genome build, then index type. |

To use a reference file in your analysis, pass its absolute path directly to your tool.
For example, the human CHM13 T2T v2.0 FASTA file is at:

```
/cvmfs/data.galaxyproject.org/byhand/CHM13_T2T_v2.0/seq/CHM13_T2T_v2.0.fa
```

> **Note:** The reference datasets available through CVMFS are those hosted by the Galaxy
> Project and may not be comprehensive. This is not a replacement for your institution's
> primary data access methods.

---

## Interactive environments {#interactive}

BioShell supports two browser-based interactive environments for notebook and script-based
work. Both run on your BioShell instance and are accessible through your browser once you
have connected via SSH.

> **Important:** You must have an active SSH connection to your BioShell instance before
> opening either environment in your browser. See [Connecting to BioShell](/access#connecting).

### JupyterLab

JupyterLab is a browser-based environment for writing and running Python, R, and shell
notebooks. It is well suited to exploratory analysis, visualisation, and combining code
with documentation.

Open your browser and go to:

```
http://<your-bioshell-ip>:8888
```

![](images/bioshell/SCREENSHOT_NEEDED_jupyterlab.png)

*Fig 4. JupyterLab open in a browser connected to a BioShell instance.*

### RStudio

RStudio is a browser-based environment for R-based analysis. If you are familiar with the
RStudio desktop application, the browser version works in exactly the same way.

Open your browser and go to:

```
http://<your-bioshell-ip>:8787
```

![](images/bioshell/SCREENSHOT_NEEDED_rstudio.png)

*Fig 5. RStudio open in a browser connected to a BioShell instance.*

> **Tip:** Your SSH terminal and browser environments connect to the same BioShell instance.
> Any tool you have loaded with `module load` in your terminal is also available inside
> JupyterLab and RStudio.

> **Note:** If you cannot reach JupyterLab or RStudio in your browser, check that your SSH
> connection is still active. If you are connecting from outside your institution's network,
> you may need an SSH tunnel. Contact your local IT support for help with this.

---

## Using Nextflow with CVMFS {#nextflow}

If you are running a Nextflow workflow, you can direct processes to use containers already
in CVMFS rather than downloading them. Create a config file that overrides the container for
the relevant process:

```groovy
process {
    withName: 'FASTQC' {
        container = 'file:///cvmfs/singularity.galaxyproject.org/all/fastqc:0.12.1--hdfd78af_0'
    }
}
```

Or use an installed SHPC module by name:

```groovy
process {
    withName: 'FASTQC' {
        module = 'fastqc/0.11.9'
    }
}
```

Run your workflow with the config override:

```bash
nextflow run main.nf -profile singularity -config cvmfs_path.config
```

> **Note:** The `module` value must match exactly what appears in `module avail`.

---

## Advanced: manual SHPC commands {#shpc-manual}

If you need to install a tool version that Bio-Shelley cannot find, or you prefer to work
directly with SHPC, use the steps below.

> **Tip:** Bio-Shelley handles most cases, including older tool versions not in the default
> registry. Try `shelley-bio versions <tool>` before attempting manual installation.

**Load SHPC:**

```bash
module load shpc
```

**Search for a tool and view versions:**

```bash
shpc show -f <toolname>
shpc show quay.io/biocontainers/<tool>
```

**Install a module directly from CVMFS:**

```bash
shpc install quay.io/biocontainers/<tool>:<tag> \
  /cvmfs/singularity.galaxyproject.org/all/<tool>:<tag> \
  --keep-path
```

**Make the module available and load it:**

```bash
module use ~/shpc/modules
module load quay.io/biocontainers/<tool>/<tag>
```

For further documentation see the
[SHPC user guide](https://singularity-hpc.readthedocs.io/en/latest/getting_started/user-guide.html).
