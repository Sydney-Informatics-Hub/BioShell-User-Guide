---
title: Tools and reference data
description: How to find, install, and load bioinformatics tools and reference datasets in BioShell using CVMFS, Shelley-Bio, and sHPC.
---

BioShell gives you access to thousands of bioinformatics tools and reference datasets through
two underlying systems — **CVMFS** and **sHPC** — and a built-in assistant called
**Shelley-Bio** that automates working with both.

[CernVM-FS (CVMFS)](https://cvmfs.readthedocs.io/en/stable/) is a read-only, network-backed
file system that delivers containerised tools and reference data on demand. Rather than
downloading or installing software yourself, you access tools as modules — files are pulled
from CVMFS only when you need them and cached locally.

BioShell mounts three CVMFS repositories automatically:

| Repository | Contents |
|-----------|----------|
| `singularity.galaxyproject.org` | 100,000+ containerised tools from [BioContainers](https://biocontainers.pro/) |
| `data.galaxyproject.org` | Reference genome builds and pre-built indexes from the Galaxy Project |
| `data.biocommons.aarnet.edu.au` | [AUTHOR TO SUPPLY — confirm final name and description] |

> **Recommended approach:** Start with Shelley-Bio. Use the manual CVMFS/sHPC methods only
> if you need finer control or are troubleshooting.

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

> **Note:** CVMFS does not download everything in advance. It fetches files only when you
> access them, so the first time you use a tool or dataset it may take a moment to load.
> Subsequent access is faster because the files are cached locally.

---

## Step 2 — Find and install tools with Shelley-Bio {#bio-shelley}

**Shelley-Bio** is BioShell's built-in assistant for finding and installing tools. It indexes
over 700 tools and 118,000 container versions from the BioContainers catalogue, so you do not
need to know container paths or sHPC commands.

You can use Shelley-Bio in two ways:

**From the command line:**

```bash
shelley-bio find <tool>
shelley-bio search "<function>"
shelley-bio versions <tool>
shelley-bio build <tool>
```

**In interactive mode:**

```bash
shelley-bio interactive
```

| Command | What it does | Example |
|---------|-------------|---------|
| `find <tool>` | Look up a specific tool by name | `shelley-bio find fastqc` |
| `search "<function>"` | Search by keyword or function | `shelley-bio search "quality control"` |
| `versions <tool>` | List all available versions | `shelley-bio versions samtools` |
| `build <tool[/version]>` | Install the tool as a loadable module | `shelley-bio build samtools/1.21` |
| `interactive` | Launch Shelley-Bio in interactive mode | `shelley-bio interactive` |

The `build` command handles the full installation automatically — it finds the correct
container in CVMFS, generates the module file, and makes the tool ready to load.

### Example: installing `samtools`

```bash
shelley-bio find samtools
# samtools — Tools for manipulating next-generation sequencing data

shelley-bio versions samtools
# samtools/1.21
# samtools/1.20
# samtools/1.19.2
# ...

shelley-bio build samtools/1.21
# Installing samtools/1.21 from CVMFS...
# Module samtools/1.21 was created.
```

![](images/bioshell/SCREENSHOT_NEEDED_bioshelley_build.png)

*Fig 3. Using `shelley-bio build samtools/1.21` to install a tool module.*

> **Tip:** Use `shelley-bio search` when you know what you want to do but not the tool name.
> For example, `shelley-bio search "variant calling"` returns tools relevant to that task.

---

## Step 3 — Load a tool {#load-tool}

Once Shelley-Bio has built a module, load it with:

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
Project. You can browse the full repository at
[datacache.galaxyproject.org](http://datacache.galaxyproject.org) before triggering any
downloads on the VM. On BioShell, the same content is available at:

```bash
ls /cvmfs/data.galaxyproject.org/byhand/
ls /cvmfs/data.galaxyproject.org/managed/
```

| Directory | Contents |
|-----------|---------|
| `/managed` | Datasets generated with Galaxy Data Manager tools. Organised by index type, then genome build. |
| `/byhand` | Older, manually curated datasets. Organised by genome build, then index type. |

To use a reference file in your analysis, pass its absolute path directly to your tool or
pipeline config. For example, the human CHM13 T2T v2.0 FASTA file is at:

```
/cvmfs/data.galaxyproject.org/byhand/CHM13_T2T_v2.0/seq/CHM13_T2T_v2.0.fa
```

The T2T genome directory also includes pre-built indexes for common aligners:

```bash
ls /cvmfs/data.galaxyproject.org/byhand/CHM13_T2T_v2.0/
# bowtie2_index/  bwa_mem_index/  bwameth_index/  hisat2_index/  len/  rnastar/  seq/
```

> **Note:** The reference datasets available through CVMFS are maintained by the Galaxy Project
> and may not be comprehensive. This is not a replacement for your institution's primary data
> access methods.

---

## Advanced: manual sHPC commands {#shpc-manual}

If you need to install a tool version that Shelley-Bio cannot find, or you prefer to work
directly with sHPC, use the steps below. This example uses `plink` throughout.

> **Tip:** Shelley-Bio handles most cases, including older tool versions not in the default
> registry. Try `shelley-bio versions <tool>` before attempting manual installation.

**Load sHPC:**

```bash
module load shpc
```

**Search for a tool and view versions:**

```bash
shpc show -f plink
shpc show quay.io/biocontainers/plink
```

**Install a module directly from CVMFS:**

```bash
shpc install quay.io/biocontainers/plink:1.90b7.7--h18e278d_1 \
  /cvmfs/singularity.galaxyproject.org/all/plink:1.90b7.7--h18e278d_1 \
  --keep-path
```

The `--keep-path` flag tells sHPC to use the container already present in CVMFS rather than
downloading a fresh copy.

**Make the module available and load it:**

```bash
module use ~/shpc/modules
module load quay.io/biocontainers/plink/1.90b7.7--h18e278d_1
plink --version
```

**Or run the container directly without a module:**

```bash
singularity exec \
  /cvmfs/singularity.galaxyproject.org/all/plink:1.90b7.7--h18e278d_1 \
  plink --version
```

For further documentation see the
[sHPC user guide](https://singularity-hpc.readthedocs.io/en/latest/getting_started/user-guide.html).

---

## Installing older or unlisted versions {#older-versions}

Some older tool versions exist in CVMFS but are not listed in the default sHPC registry.
Shelley-Bio handles this automatically via its `build` command, but if you need to do it
manually, follow these steps.

**1. Check CVMFS for the version:**

```bash
ls /cvmfs/singularity.galaxyproject.org/all/plink:1.90b4*
```

**2. Get its checksum:**

```bash
sha256sum /cvmfs/singularity.galaxyproject.org/all/plink:1.90b4--h0a6d026_2
```

**3. Create a local registry entry:**

```bash
sudo mkdir -p /apps/local/quay.io/biocontainers/plink

# Fetch the existing remote recipe as a base
curl -fsSL \
  https://raw.githubusercontent.com/singularityhub/shpc-registry/main/quay.io/biocontainers/plink/container.yaml \
  -o /apps/local/quay.io/biocontainers/plink/container.yaml

# Edit the YAML to add the missing version tag and its checksum
```

**4. Register your local registry:**

```bash
sudo shpc config add registry /apps/local
```

Verify your local registry appears first — it must take precedence over the remote registry:

```bash
shpc config get registry
# ['/apps/local', 'https://github.com/singularityhub/shpc-registry']
```

**5. Install from CVMFS:**

```bash
sudo shpc install quay.io/biocontainers/plink:1.90b4--h0a6d026_2 \
  /cvmfs/singularity.galaxyproject.org/all/plink:1.90b4--h0a6d026_2 \
  --keep-path
```

> **Tip:** If Shelley-Bio recognises the container path but the version is not in the
> registry, `shelley-bio build` handles the local registry creation automatically.

---

## Troubleshooting {#troubleshooting}

**CVMFS probe fails for a repository**

The repository may be temporarily unavailable. Wait a moment and run `cvmfs_config probe`
again. Contact [Australian BioCommons support](https://www.biocommons.org.au/helpdesk) if
the problem persists.

**`shpc show` returns no results**

The default registry may not include every BioContainers tool. Try searching the remote
registry explicitly:

```bash
shpc show quay.io/biocontainers/<tool> \
  --registry https://github.com/singularityhub/shpc-registry
```

The official registry is updated nightly.

**Module appears in `module avail` but will not load**

Check that both `shpc` and `singularity` are loaded:

```bash
module list
```

sHPC requires Singularity to execute containers.

**Shelley-Bio cannot find a tool**

Try `search` with different keywords — for example, `shelley-bio search "alignment"` instead
of a specific tool name. If the container exists in CVMFS but Shelley-Bio does not index it,
fall back to the manual sHPC method above.

---

## Further reading {#further-reading}

- [sHPC user guide](https://singularity-hpc.readthedocs.io/en/latest/getting_started/user-guide.html)
- [BioContainers registry](https://biocontainers.pro/registry)
- [Shelley-Bio on GitHub](https://github.com/Sydney-Informatics-Hub/shelley-bio)
- [CVMFS documentation](https://cvmfs.readthedocs.io/en/stable/)
- [Galaxy Project CVMFS repositories](https://galaxyproject.org/admin/cvmfs/)
