---
title: Tools and reference data
description: How to find, install, and load bioinformatics tools and reference datasets in BioShell using CVMFS, Bio-Shelley, and SHPC.
---

BioShell gives you access to thousands of bioinformatics tools and reference datasets through
[CernVM-FS (CVMFS)](https://cvmfs.readthedocs.io/en/stable/), a read-only shared file system
that delivers containerised tools and reference data on demand. Rather than downloading or
installing software yourself, you load tools as modules — they are pulled from CVMFS only when
you need them.

Through CVMFS, BioShell connects to three repositories:

| Repository | Contents |
|-----------|----------|
| `singularity.galaxyproject.org` | Containerised tools from [BioContainers](https://biocontainers.pro/) |
| `data.galaxyproject.org` | Reference genome builds and pre-built indexes from the Galaxy Project |
| `data.biocommons.aarnet.edu.au` | [AUTHOR TO SUPPLY — confirm final name and description] |

> **Note:** CVMFS does not download everything in advance. It fetches files only when you
> access them, so the first time you use a tool or dataset it may take a moment to load.
> Subsequent access is faster because the files are cached locally.

---

## Step 1 — Verify your CVMFS connection {#cvmfs}

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

---

## Step 2 — Find and install tools with Bio-Shelley {#bio-shelley}

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

---

## Step 3 — Load a tool {#load-tool}

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
