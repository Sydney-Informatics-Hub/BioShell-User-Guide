---
title: How to use Shelley
type: Using BioShell
description: A collection of guides for finding, building, and installing bioinformatics tools with Shelley
---

This page collects several use cases for Shelley to find and build tools. If you are new to Shelley and want a guided tour of how
it fits together, see the [**Getting started with Shelley**](tools#getting-started-with-shelley)
tutorial first.

## Basic usage {#basic-usage}

**From the command line:**

```bash
shelley find <tool> # Look up a specific tool by name
shelley search "<function>" # Search by keyword or function
shelley build <tool> # Install the tool as a loadable module
```

**In interactive mode:**

```bash
shelley interactive # Launch Shelley in interactive mode
```

Interactive mode works the same way as the command line. The `find`, `search`, and `build`
behave identically, except you type just the command name and its arguments, without
prefixing every call with `shelley`.

## How-to install `bwa-mem2` {#how-to-install-bwa-mem2}

This worked example uses `bwa-mem2` to demonstrate the steps to use and apply to any tool in the CVMFS catalogue.

**1.** Use `shelley` to look up `bwa-mem2` and the versions available for it

```bash
shelley find bwa-mem2
```

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_find_bwa-mem2.png)
<br>
</details>
<br>

**2.** Build a loadable module

```bash
shelley build bwa-mem2/2.3
```

<details markdown="1">
<summary>Show output</summary>
![](assets/img/shelley_build_bwa-mem2.png)
<br>
</details>
<br>

**3.** Confirm the module was built succesfully

```bash
module avail
```

<details markdown="1">
<summary>Example output</summary>
![](assets/img/module_avail.png)
<br>
</details>
<br>

**4.** Load the module

```bash
module load bwa-mem2
```

**5.** Finally, run `bwa-mem2`

```bash
bwa-mem2 version
# 2.2.1
```

You now have a working `bwa-mem2` module built from the BioContainers catalogue.
The same five steps: find, build, confirm, load, run - apply to any tool available
through Shelley.

{% include callout.html type="note" content="Curious why Shelley automates this instead of running `shpc install` by hand? See [why Shelley over manual sHPC](https://github.com/Sydney-Informatics-Hub/shelley/blob/main/docs/explanation/why-shelley.md) in the Shelley docs." %}

## How-to bulk install a list of containers {#bulk-install}

If you need several modules built at once, for example, all the tools a pipeline
depends on, you can pass `shelley build` a plain-text file listing one tool spec per line
instead of building each tool individually.

**1.** Create `tools.txt` with the following contents

```md
# qc tools
fastqc

# Core alignment tools
# some tools have versions specified for reproducibility
samtools/1.21
bowtie2/2.5.1
bwa-mem2
```

Each line accepts the same format as a single-tool `build` call:
- `<tool>`,
- `<tool>/<version>`, or
- `<tool>:<version>--<hash>`

Blank lines and `#` comments are ignored, so you can group and annotate the list.

**2.** Run the `build` command:

```bash
shelley build tools.txt
```

Shelley builds each tool in the file in sequence, showing a progress table as it goes.

<details markdown="1">
<summary>Show output</summary>
![](assets/img/shelley_build_tools-txt.png)
<br>
</details>
<br>

**3.** If a tool has more than one build for the same version (for example, several
`--hash` builds of `samtools/1.21`), Shelley pauses and asks you to pick one.

<details markdown="1">
<summary>Show output</summary>
![](assets/img/shelley_build_multiple-builds-prompt.png)
<br>
</details>
<br>

Use the arrow keys to select a build, then press Enter to continue the batch.

**4.** Once every tool has been built, Shelley prints a summary, and the modules are
available to load.

```bash
module avail
```

<details markdown="1">
<summary>Show output</summary>
![](assets/img/shelley_build_tools-txt_summary.png)
<br>
</details>
<br>

```
---------------------------------------------------------- /apps/Modules/modulefiles ----------------------------------------------------------
   R/4.3.3           bowtie2/2.5.1--py39h6fed5c7_2    fastqc/0.12.1--hdfd78af_0    nextflow/25.10.4    rstudio/2023.12.1
   ansible/2.16.3    bwa-mem2/2.3--he70b90d_0         jupyter/2026.04              nf-core/3.5.2       samtools/1.21--h96c455f_1

---------------------------------------------------------- /opt/Modules/modulefiles -----------------------------------------------------------
   shpc (L)    singularity (L)

  Where:
   L:  Module is loaded
```

## How-to find the full CVMFS path of containers {#find-cvmfs-path}

Some workflows need the exact CVMFS container path rather than a loaded module. For
example, a Nextflow process configured to pull a container directly instead of using
`module load`. Use `find -v` to to list every individual build of a tool together
with its full CVMFS image path:

```bash
shelley find fastqc -v
```

<details markdown="1">
<summary>Show output</summary>
![](assets/img/shelley_find_fastqc_v.png)
<br>
</details>
<br>

Each row corresponds to one `--hash` build of a version, with its buildable/installed
status and the path you can copy into a config file, such as:

```
/cvmfs/singularity.galaxyproject.org/all/fastqc:0.12.1--hdfd78af_0
```

See [Using Nextflow with CVMFS](nextflow) for a worked example of pointing a Nextflow
process at a container path like this.

## How-to build a tool that isn't in the sHPC registry {#unregistered-tool}

Building an Lmod module requires a registry entry describing the container.
sHPC's [shpc-registry](https://github.com/singularityhub/shpc-registry) supplies these
for most tools, but it doesn't cover every version. Very new containers, and containers
built before a tool was added to the registry, often have no entry at all.

**You don't need to know whether a version is registered as `shelley build` checks for you
and will build this on-the-fly.**

```bash
shelley build bowtie2/2.5.1
```

Shelley diffs the container against its base-image manifests to work out which files
are the tool's own, so the module's aliases don't get cluttered with every binary
bundled inside the container:

<details markdown="1">
<summary>Example output</summary>
```
Generating diff for /cvmfs/singularity.galaxyproject.org/all/bowtie2:2.5.1--py39h6fed5c7_2
<truncated>
docker://docker.io/anaconda/miniconda:latest: removed 98 shared paths.
docker.io/library/busybox:1.34: removed 90 shared paths.
<truncated>
```

![](assets/img/shelley_build_bowtie2_unregistered.png)
<br>
</details>
<br>

If the version is missing from the upstream registry, Shelley creates a local registry
entry from the CVMFS container itself and retries the install. This is transparent and
adds only a few extra minutes to the build. See [Why Shelley over manual sHPC](https://github.com/Sydney-Informatics-Hub/shelley/blob/main/docs/explanation/why-shelley.md)
for what this replaces manually, and [Build design](https://github.com/Sydney-Informatics-Hub/shelley/blob/main/docs/explanation/build-design.md)
for how the fallback works internally.

{% include callout.html type="note" content="Because the alias list for a locally-registered entry is generated straight from the container, it can include extra utility binaries bundled alongside the tool (for example, conda-infrastructure commands) rather than only the tool's own executables." %}

## [EXPERIMENTAL] How-to amend the aliases {#amend-aliases}

{% include callout.html type="important" content="Experimental feature - use with caution. Curating aliases changes which commands a module exposes; an alias you remove or rename may break other users' or pipelines' expectations of that module." %}

As noted [above](#unregistered-tool), a container can bundle utility binaries alongside
the tool itself, which can clutter the module's alias list with commands you don't
need. Add `-i` (or `--interactive`) to `build` to open a session where you can curate
the aliases a module exposes such as deselecting, renaming, or adding them; before the install
completes:

```bash
shelley build vcftools/0.1.12b--pl5262h2e03b76_2 -i
```

**1.** Shelley first lists every binary in the container as a candidate alias — without
curation, all of these would be exposed by the module:

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_build_vcftools_alias-select-all.png)
<br>
</details>
<br>

**2.** Type to filter the list down to the ones relevant to `vcftools`, for example `vcf`:

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_build_vcftools_alias-filter-vcf.png)
<br>
</details>
<br>

**3.** Toggle each one you want with space, then press Enter once they're all selected:

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_build_vcftools_alias-selected.png)
<br>
</details>
<br>

**4.** Shelley then asks whether to add or rename any aliases. Answer `n` to both to keep
the selection as-is:

```
? Add new aliases? (y/n)No
? Rename any aliases? (y/n)No
```

**5.** The module builds with only the curated aliases exposed:

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_build_vcftools_complete.png)
<br>
</details>
<br>

