---
title: Tools and reference data
type: Using BioShell
description: How to find, install, and load bioinformatics tools and reference datasets on BioShell using Shelley.
---

BioShell instances arrive with bioinformatics software and some reference data already installed. You don't have to compile tools, manage dependencies, or track down
container images before you can start.

## What's installed {#preinstalled}

Every instance comes with a core set of tools:

| Tool | Purpose |
|------|---------|
| **Python 3** | General-purpose scripting, and the language much of bioinformatics is built on |
| **R** | Statistical analysis and visualisation |
| **[JupyterLab](interactive#jupyterlab)** | Browser-based notebooks holding code, plots, and notes in one place |
| **[RStudio](interactive#rstudio)** | Browser-based development environment for R |
| **[Nextflow](nextflow-howto)** | Runs reproducible, scalable analysis pipelines |
| **[nf-core](nextflow-howto#nfcore)** | Utilities and configurations for running nf-core pipelines |
|**[Globus Connect Personal](globus)**| Make your VM a Globus end point for easy data movement |

Some of these are on your `PATH` and ready to type; others are modules you load first. To see
everything available on your instance:

```bash
module avail
```

Then load what you need, for example `module load jupyter` or `module load rstudio`. Anything
not listed, you can install yourself with [**Shelley**](tutorials/shelley-howto).

## How BioShell manages bioinformatics software {#tooling-stack}

Bioinformatics often requires us to use many different software including command-line software, R and Python packages. BioShell gives you access to over 100,000 bioinformatics packages, managed in three layers for you:

- **[CernVM-FS](https://cvmfs.readthedocs.io/en/stable/)** is a read only filesystem that acts as a repository for 13,000+ tools and 118,000+
  versions from [BioContainers](https://biocontainers.pro/registry). It is available in your BioShell VM at `/cvmfs/`. It looks like an ordinary folder, and files
  are fetched only when you use them, to help you manage your disk space.
- **[sHPC](https://singularity-hpc.readthedocs.io/)** packages containers in cvmfs into installable modules
- **[Lmod](https://lmod.readthedocs.io/en/latest/)** is the system behind the `module` command you use to load and switch tools

You don't need to undersand any of this to use BioShell because **Shelley**, BioShell's command-line assistant, drives all three for you. She:

* Searches the tool library
* Picks the right container version
* Creates any sHPC registry entry that is missing
* Installs the module: one command to find a tool, one to install it

![](assets/img/shelley-orchestrator.png)

## Finding and installing tools with Shelley :turtle: {#getting-started-with-shelley}

Shelley runs from the command line or in an interactive mode. This walkthrough installs a tool
on a BioShell VM for the first time. Start by confirming Shelley is available:

```bash
shelley help
```

You will see a list of available commands.

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_help.png)
<br>
</details>
<br>

### Find a tool you know by name

Say you already know you need `fastqc`. Look it up with `find`:

```bash
shelley find fastqc
```

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_find_fastqc.png)
<br>
</details>
<br>

Shelley returns the tool's description, its most recent container versions, and whether it is
installed as a module yet. `find` is forgiving about naming: case, hyphens, and underscores are
all handled for you, so `shelley find STAR`, `shelley find bwa-mem2`, and `shelley find samtools`
all work as expected.

### See every available version

By default `find` shows only the most recent versions of a tool. To pin an exact version for
reproducibility, or to match a pipeline's requirements, add the `-v` (verbose) flag to see every
available container, newest first:

```bash
shelley find fastqc -v
```

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_find_fastqc_v.png)
<br>
</details>
<br>


### Search when you only know the task

Sometimes you know what you want to do but not which tool does it. That's what `search` is for:

```bash
shelley search "quality control"
shelley search "variant calling"
shelley search "de novo assembly"
```

Each result shows the tool name and a brief description of what it does. **Shorter, more
specific phrases work better than full sentences** — every extra word broadens the match rather
than narrowing it, so remove words rather than adding them if you get too many results.

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_search_de-novo-assembly.png)
<br>
</details>
<br>

{% include callout.html type="note" content="Search is under active development. Results are broad and currently presented alphabetically." %}


### Build the module

Once you know the tool and version you want, build its module with `shelley build`:

```bash
shelley build fastqc
```

This installs the most recent available version by default.

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_build_fastqc.png)
<br>
</details>
<br>

{% include callout.html type="tip" content="To install a specific version instead of the most recent one, give `build` the same `<tool>/<version>` spec that `find -v` showed you, for example `shelley build fastqc/0.12.1`." %}

### Load and run the tool

Load the module the same way you would on any HPC system, then run the tool:

```bash
module load fastqc
fastqc --version
# FastQC v0.12.1
```

That's the whole loop, and it is the same for every tool: find, build, load, run. When you are
ready for more, [**How to use Shelley**](shelley-howto) covers the other use cases that will
come in handy.


## Reference genomes and indexes {#reference-data}

Reference genome builds and pre-built indexes, managed and maintained by the
[Galaxy Project](https://galaxyproject.org/admin/cvmfs/), sit in two directories:

```bash
ls /cvmfs/data.galaxyproject.org/byhand/    # by genome build, then index type
ls /cvmfs/data.galaxyproject.org/managed/   # by index type, then genome build
```

{% include callout.html type="note" content="The reference datasets available through CVMFS are maintained by the Galaxy Project and may not be comprehensive. This is not a replacement for your institution's primary data access methods." %}


## Troubleshooting {#troubleshooting}

**CVMFS probe fails for a repository**

The repository may be temporarily unavailable. Wait a moment and run `cvmfs_config probe`
again. Contact [Australian BioCommons support](https://www.biocommons.org.au/helpdesk) if
the problem persists.

**Module appears in `module avail` but will not load**

Check that both `shpc` and `singularity` are loaded:

```bash
module list
```

sHPC requires Singularity to execute containers.

**Shelley cannot find a tool**

Try `search` with broader keywords, for example `shelley search "alignment"` instead of a
specific tool name. If the container exists in CernVM-FS but Shelley does not index it, install
the module manually with `shpc install` — see the
[sHPC user guide](https://singularity-hpc.readthedocs.io/en/latest/getting_started/user-guide.html).


## Further reading {#further-reading}

- [**How to use Shelley**](shelley-howto) — snippets for various use cases
- [Full CLI reference](https://github.com/Sydney-Informatics-Hub/shelley/blob/main/docs/reference/cli.md)
- [Design rationale](https://github.com/Sydney-Informatics-Hub/shelley/tree/main/docs/explanation)
- [BioContainers registry](https://biocontainers.pro/registry)
- [Galaxy Project CVMFS repositories](https://galaxyproject.org/admin/cvmfs/)
- [CVMFS documentation](https://cvmfs.readthedocs.io/en/stable/)
- [sHPC user guide](https://singularity-hpc.readthedocs.io/en/latest/getting_started/user-guide.html)
