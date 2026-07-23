---
title: How to use Shelley
type: Using BioShell
description: Task-oriented steps for finding, building, and installing bioinformatics tools with Shelley
---

TODO: This is a collection of end-to-end how-to guides for using Shelley's functions

If you are new to Shelley and want a guided tour of how
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

TODO: in interactive mode, it works the same way but you don't have to use the `shelley` command every time.

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

## How-to bulk install a list of containers

TODO:
- To do this, pass in a list of text files

## How-to find the full CVMFS path of containers

TODO:
- Use cases where you need to reference the full container path e.g. nextflow.config
- Add the previous nextflow.config example from @tools.md here
- Use find -vv
- Leave placeholders for screenshots

## How-to build modules without an unregistrered tool

TODO:
- Containers require a registry file to build the module, often this doesn't require 
- You will not know this, and it doesn't matter because shelley deals with this on the fly
- These include very new containers, or containers that were built prior to the registry
- Downside: May clutter with utility binaries that you don't need

## [EXPERIMENTAL] How-to amend the aliases 

TODO:
- use with caution
- For example, container may include a lot of utility binaries that are not required
- You can manually amend this using `build -i <container>`
- vcftools example user will provide.

