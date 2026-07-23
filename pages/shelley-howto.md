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

1. Use `shelley` to look up `bwa-mem2` and the versions available for it:

```bash
shelley find bwa-mem2
```

<details markdown="1">
<summary>Example output</summary>
![](assets/img/shelley_find_bwa-mem2.png)
<br>
</details>


2. Build a loadable module:

```bash
shelley build bwa-mem2/2.3
```

<details markdown="1">
<summary>Show output</summary>
![](assets/img/shelley_build_bwa-mem2.png)
<br>
</details>

3. Confirm the module was built succesfully:

```bash
module avail
```

<details markdown="1">
<summary>Example output</summary>
![](assets/img/module_avail.png)
<br>
</details>

4. Load the module:

```bash
module load bwa-mem2
```

5. Finally, run `bwa-mem2`:

```bash
bwa-mem2 version
# 2.2.1
```

You now have a loadable `bwa-mem2` module built from the BioContainers catalogue.
The same five steps: find, build, confirm, load, run - apply to any tool available
through Shelley.

{% include callout.html type="note" content="Curious why Shelley automates this instead of running `shpc install` by hand? See [why Shelley over manual sHPC](https://github.com/Sydney-Informatics-Hub/shelley/blob/main/docs/explanation/why-shelley.md) in the Shelley docs." %}

## How-to bulk install a list of containers {#bulk-install}

If you need several modules built at once, for example, all the tools a pipeline
depends on, you can pass `shelley build` a plain-text file listing one tool spec per line
instead of building each tool individually:

```bash
shelley build tools.txt
```

Each line follows the same format as a single-tool `build` call — `<tool>`,
`<tool>/<version>`, or `<tool>:<version>--<hash>`. Blank lines and `#` comments are
ignored, so you can group and annotate the list:

```
# Core alignment tools
samtools/1.21
bwa
bowtie2/2.5.1  # pinned for reproducibility
fastqc
```

Shelley detects that the argument is a file rather than a tool name, and builds every
entry in sequence, showing a progress table and a results summary at the end.

## How-to find the full CVMFS path of containers {#find-cvmfs-path}

Some workflows need the exact CVMFS container path rather than a loaded module. For
example, a Nextflow process configured to pull a container directly instead of using
`module load`. Use `find -v` to to list every individual build of a tool together
with its full CVMFS image path:

```bash
shelley find fastqc -v
```

TODO: screenshot

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

You don't need to know whether a version is registered as `shelley build` checks for you
and will build this for you on-the-fly:

```bash
shelley build <tool>/<version> #TODO later: add version
```

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
shelley build <tool>/<version> -i
```

TODO: user to add a worked example (vcftools)

