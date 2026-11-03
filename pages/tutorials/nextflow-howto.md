---
title: Using Nextflow on BioShell
type: Tutorial
description: Learn to run a Nextflow pipeline on BioShell using containers already available in CVMFS, with nf-core/scrnaseq as a worked example.
---

When Nextflow runs a pipeline with containers, it normally downloads every image it needs
before each process can start. On BioShell, most of these containers already exist in CVMFS,
so downloading them again wastes both time and disk space. Pointing your pipeline at the
CVMFS copies avoids both.

In this tutorial, you will run [nf-core/scrnaseq](https://nf-co.re/scrnaseq) on its minimal
test data. Along the way you will work out which containers the pipeline actually needs,
swap as many as possible for CVMFS paths, and collect those changes into a single
`bioshell.config` file. By the end, you will have a completed pipeline run, and a pattern you
can reuse for any Nextflow pipeline.

You can follow along with the video below, or work through the steps on this page.

<!-- TODO: replace VIDEO_URL with the embed link for the walkthrough video -->
<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; margin-bottom: 1rem;">
  <iframe src="VIDEO_URL" title="Using Nextflow on BioShell" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border: 0;" allowfullscreen></iframe>
</div>

## Before you start {#before-you-start}

You will need:

- A BioShell instance with an attached data volume. This tutorial assumes it is mounted at
  `/mnt/data`; replace this with your own mount point if it differs.
- Basic familiarity with [Shelley](shelley-howto), which you will use to look up container paths.

You don't need to have used Nextflow before, as each command is explained as it comes up. If
you'd like more background, the Sydney Informatics Hub's
[Nextflow for the Life Sciences](https://sydney-informatics-hub.github.io/nf4ls-materials/)
materials cover the fundamentals, and
[Unlocking nf-core](https://sydney-informatics-hub.github.io/customising-nfcore-workshop-2026/)
covers running and customising nf-core pipelines like the one in this tutorial.

## 1. Prepare your BioShell {#prepare}

First, set up a working area with the tools, the pipeline code, and the test data.

**1.** Move into your data volume

```bash
cd /mnt/data
```

Everything in this tutorial happens inside this directory.

{% include callout.html type="note" content="Bioinformatics pipelines read and write large files: input reads, reference genomes, intermediate files in the `work/` directory, and any containers that need to be downloaded. Your home directory sits on the instance's root disk, which is small and fills up quickly. Keeping everything on the data volume avoids running out of space partway through a run." %}

**2.** Load the Nextflow and Singularity modules

```bash
module load nextflow singularity
```

Nextflow orchestrates the pipeline: it decides what runs, in what order, and with which
inputs. Singularity runs each individual tool inside its container.

**3.** Check that CVMFS is reachable, so Nextflow can access the container images

```bash
cvmfs_config probe
```

Each CVMFS repository should report `OK`.

**4.** Clone the pipeline code

```bash
git clone --branch 4.2.0 https://github.com/nf-core/scrnaseq.git
```

Keeping a local copy of the pipeline means you always know exactly which files live where and
what is being run. Pinning a release (here, `4.2.0`) also means the container versions you see
will match the ones shown in this tutorial. Run `ls` to check that a `scrnaseq/` folder has
appeared.

**5.** Download the test data into a `data/` folder

These are the same small files nf-core uses to test the pipeline: a handful of 10x reads from
two samples, plus a mouse chromosome 19 reference and annotation. They are designed to run
quickly, rather than for real biological analysis.

{% include callout.html type="tip" content="When you run the pipeline on your own data, use [Globus](globus-howto) to transfer it onto your data volume. It handles large files and resumes interrupted transfers, which `wget` and `scp` don't." %}

```bash
mkdir data && cd data
wget https://raw.githubusercontent.com/nf-core/test-datasets/scrnaseq/testdata/cellranger/Sample_X_S1_L001_R1_001.fastq.gz
wget https://raw.githubusercontent.com/nf-core/test-datasets/scrnaseq/testdata/cellranger/Sample_X_S1_L001_R2_001.fastq.gz
wget https://raw.githubusercontent.com/nf-core/test-datasets/scrnaseq/testdata/cellranger/Sample_Y_S1_L001_R1_001.fastq.gz
wget https://raw.githubusercontent.com/nf-core/test-datasets/scrnaseq/testdata/cellranger/Sample_Y_S1_L001_R2_001.fastq.gz
wget https://raw.githubusercontent.com/nf-core/test-datasets/scrnaseq/testdata/cellranger/Sample_Y_S1_L002_R1_001.fastq.gz
wget https://raw.githubusercontent.com/nf-core/test-datasets/scrnaseq/testdata/cellranger/Sample_Y_S1_L002_R2_001.fastq.gz
wget https://github.com/nf-core/test-datasets/raw/scrnaseq/reference/GRCm38.p6.genome.chr19.fa
wget https://github.com/nf-core/test-datasets/raw/scrnaseq/reference/gencode.vM19.annotation.chr19.gtf
cd ..
```

**6.** Create `samplesheet.csv` pointing to the reads

```
sample,fastq_1,fastq_2,protocol,expected_cells
Sample_X,data/Sample_X_S1_L001_R1_001.fastq.gz,data/Sample_X_S1_L001_R2_001.fastq.gz,10XV2,5000
Sample_Y,data/Sample_Y_S1_L001_R1_001.fastq.gz,data/Sample_Y_S1_L001_R2_001.fastq.gz,10XV2,5000
Sample_Y,data/Sample_Y_S1_L002_R1_001.fastq.gz,data/Sample_Y_S1_L002_R2_001.fastq.gz,10XV2,5000
```

The samplesheet tells the pipeline which reads belong to which sample. `Sample_Y` was
sequenced across two lanes, so it appears twice.

## 2. Find which processes will run {#preview}

A pipeline usually contains far more processes than any single run uses. nf-core/scrnaseq,
for example, supports several aligners (STAR, simpleaf, Kallisto, Cell Ranger), but this run
only uses STAR. Rather than sorting out containers for every process in the pipeline, you
will first find the ones that will actually run.

**1.** Create a run script, `run.sh`, with the pipeline parameters and the `-preview` flag

```bash
#!/bin/bash

nextflow run scrnaseq \
  --input samplesheet.csv \
  --outdir results \
  --aligner star \
  --protocol 10XV2 \
  --fasta data/GRCm38.p6.genome.chr19.fa \
  --gtf data/gencode.vM19.annotation.chr19.gtf \
  -profile singularity \
  -preview
```

The `--` options are the pipeline's own parameters: the samplesheet, where to write results,
which aligner to use, and the reference files. The single-dash options belong to Nextflow
itself. `-profile singularity` tells the pipeline to run its tools with Singularity, and
`-preview` does a dry run: Nextflow works out which processes your parameters will trigger and
lists them, without running any tools or touching your data.

Keeping the command in a script makes it easy to edit and rerun, which you will do several
times in this tutorial.

**2.** Run the script

```bash
bash run.sh
```

After a few seconds, Nextflow reports that the pipeline completed successfully and lists the
processes it would run.

**3.** Copy the listed process names into `processes.txt` so you can refer back to them

```
FASTQC_CHECK:FASTQC
PREPARE_GENOME:GTF_GENE_FILTER
STARSOLO:STAR_GENOMEGENERATE
STARSOLO:STAR_ALIGN
MTX_TO_H5AD
H5AD_CONVERSION:CONCAT_H5AD
H5AD_CONVERSION:ANNDATAR_CONVERT
MULTIQC
```

The name after the last `:` is the process name, and anything before it is the workflow or
subworkflow it belongs to. Notice that only the STAR processes appear. The simpleaf, Kallisto,
and Cell Ranger processes are skipped, so you won't need their containers. These eight
processes are the ones you will find containers for.

{% include callout.html type="note" content="`nextflow inspect` (the next section) lists the container for every process in the pipeline, whether it will run or not. `-preview` lists only the processes your parameters will trigger, but not their containers. At the time of writing, no single command gives you both, so you use `-preview` to narrow down the list and `inspect` to get the container details." %}

## 3. List the containers the pipeline uses {#inspect}

Now that you know which processes will run, the next step is finding out which container
each one uses. This list becomes the starting point for `bioshell.config`.

**1.** In `run.sh`, replace `run` with `inspect`, and replace `-preview` with
`-format config > bioshell.config`

```bash
#!/bin/bash

nextflow inspect scrnaseq \
  --input samplesheet.csv \
  --outdir results \
  --aligner star \
  --protocol 10XV2 \
  --fasta data/GRCm38.p6.genome.chr19.fa \
  --gtf data/gencode.vM19.annotation.chr19.gtf \
  -profile singularity \
  -format config > bioshell.config
```

`inspect` doesn't run anything either. It prints the container each process is configured to
use, and `-format config` prints them in Nextflow's config syntax, so the output is already
most of the way to a usable config file.

**2.** Run the script

```bash
bash run.sh
```

Open `bioshell.config`. Each line gives a process name and the container it uses, including
the tool name and version:

```groovy
process {
    withName: 'FASTQC' {
        container = 'https://depot.galaxyproject.org/singularity/fastqc:0.12.1--hdfd78af_0'
    }
    withName: 'CELLRANGER_MULTI' {
        container = 'quay.io/nf-core/cellranger:10.0.0'
    }
    ...
}
```

**3.** Trim `bioshell.config` down to the processes in `processes.txt`, and wrap them in a
single `process { }` block

Working through `processes.txt`, keep the matching line for each process and delete the rest,
along with any duplicates. Check that you end up with the same number of processes in both
files (eight here):

```groovy
process {
    withName: 'FASTQC' {
        container = 'https://depot.galaxyproject.org/singularity/fastqc:0.12.1--hdfd78af_0'
    }
    withName: 'GTF_GENE_FILTER' {
        container = 'https://depot.galaxyproject.org/singularity/python:3.9--1'
    }
    withName: 'STAR_GENOMEGENERATE' {
        container = 'https://community-cr-prod.seqera.io/docker/registry/v2/blobs/sha256/26/268b4c9c6cbf8fa6606c9b7fd4fafce18bf2c931d1a809a0ce51b105ec06c89d/data'
    }
    withName: 'STAR_ALIGN' {
        container = 'https://depot.galaxyproject.org/singularity/star:2.7.10b--h9ee0642_0'
    }
    withName: 'MTX_TO_H5AD' {
        container = 'community.wave.seqera.io/library/scanpy:1.10.2--e83da2205b92a538'
    }
    withName: 'CONCAT_H5AD' {
        container = 'community.wave.seqera.io/library/scanpy:1.10.2--e83da2205b92a538'
    }
    withName: 'ANNDATAR_CONVERT' {
        container = 'https://community-cr-prod.seqera.io/docker/registry/v2/blobs/sha256/d8/d8036a262b63ab783572e3cb3120b6b138593e19d93a1059b9bfef80785c7218/data'
    }
    withName: 'MULTIQC' {
        container = 'https://community-cr-prod.seqera.io/docker/registry/v2/blobs/sha256/1b/1bef8af6be88c5733461959c46ac8ef73d18f65277f62a1695d0e1633054f9c2/data'
    }
}
```

The key piece of information here is the version of each tool, which you will use to find the
matching container in CVMFS.

## 4. Swap in CVMFS containers {#swap-containers}

Looking at the list above, you'll notice the containers come from several different online
registries. How you replace each one depends on where it is hosted, so you will work through
them one registry at a time.

### Which containers can be replaced? {#which-containers}

CVMFS mirrors the Galaxy Project's Singularity images, and also carries many other single-tool
containers. Whether a container can be swapped depends on how easily you can tell which tool
and version it holds, and whether it bundles more than one tool:

| Registry | Example | Can it use CVMFS? |
|---|---|---|
| Galaxy Project (`depot.galaxyproject.org`) | `fastqc:0.12.1--hdfd78af_0` | Yes. CVMFS has the identical image, so it is a direct swap. |
| Seqera Wave (`community.wave.seqera.io`) | `scanpy:1.10.2--e83da2205b92a538` | Often. The tool and version are in the name, so you can look for a match. |
| Seqera community (`community-cr-prod.seqera.io`) | `.../sha256/26/268b4c.../data` | Sometimes. The name is only a hash, so you first need to check which tool it contains. |
| Mulled (multi-tool) containers | `htslib_samtools_star_gawk:...` | No. CVMFS has single-tool containers, so leave these as they are. |

The sections below work through each registry in turn, starting with the direct swaps.

{% include callout.html type="tip" content="CVMFS paths must start with `file://` (for example `file:///cvmfs/...`), because Singularity runs the image directly from disk. Leave containers that must be downloaded as `https://`." %}

### Galaxy Project containers

The containers in CVMFS are exactly the same as the Galaxy Project ones, so these are the
easiest to replace. You only need to find the matching path.

**1.** Look up the CVMFS path for the matching version with `shelley find -v`

```bash
shelley find fastqc -v
```

<details markdown="1">
<summary>Show output</summary>
![](assets/img/shelley_find_fastqc_v.png)
<br>
</details>
<br>

Shelley lists the most recent FastQC containers, each with its version, build hash, and full
CVMFS path. The pipeline uses `0.12.1--hdfd78af_0`, which is at the top.

**2.** Copy that path into `bioshell.config`, adding the `file://` prefix

```groovy
    withName: 'FASTQC' {
        container = 'file:///cvmfs/singularity.galaxyproject.org/all/fastqc:0.12.1--hdfd78af_0'
    }
```

**3.** Repeat for the other Galaxy Project containers, `GTF_GENE_FILTER` (Python 3.9) and
`STAR_ALIGN` (STAR 2.7.10b)

```bash
shelley find python -v
shelley find star -v
```

### Seqera Wave containers

Wave container names include the tool and version, so you can search for a match the same
way. `MTX_TO_H5AD` and `CONCAT_H5AD` both use ScanPy 1.10.2:

```bash
shelley find scanpy -v
```

This time, the newest version in CVMFS is 1.7, which is older than the version the pipeline
expects. This is where you have to make a judgement call. For this pipeline, the newer version
matters, so **leave both ScanPy processes out of `bioshell.config`**. Without an entry in your
config, Nextflow uses the pipeline's own containers for them.

{% include callout.html type="warning" content="Using the older ScanPy 1.7 for `MTX_TO_H5AD` produces `.h5ad` files that the downstream `ANNDATAR_CONVERT` process can't read, and the run fails. In the recording, we swapped both ScanPy processes to the CVMFS 1.7 container: most of the pipeline finished, but `ANNDATAR_CONVERT` reported that the `.h5ad` file was invalid or written by an incompatible version. Deleting the two ScanPy entries from `bioshell.config` and re-running with [`-resume`](#resume) finished the remaining processes without repeating the ones that had already succeeded." %}

{% include callout.html type="note" content="**Should I use an older version from CVMFS?** It depends on how much the versions differ and how much reproducibility matters to you. Check the tool's changelog between the two versions: if nothing relevant to your analysis has changed, the CVMFS version saves a download. If file formats, defaults, or algorithms have changed, or you need to match the pipeline exactly, keep the pipeline's container." %}

### Seqera community containers

The remaining three containers are the trickiest, because their names are only a hash. Before
you can look for a match, you need to find out what's inside them.

**1.** Inspect the pipeline again, this time with the Docker profile

```bash
nextflow inspect scrnaseq -profile docker -format config | grep -E 'STAR_GENOMEGENERATE|ANNDATAR_CONVERT|MULTIQC'
```

The Docker profile refers to the same containers by readable names, and `grep` filters the
output down to the three processes you're interested in:

```groovy
process {
    withName: 'MULTIQC' {
        container = 'community.wave.seqera.io/library/multiqc:1.34--db7c73dae76bc9e6'
    }
    withName: 'ANNDATAR_CONVERT' {
        container = 'community.wave.seqera.io/library/bioconductor-anndatar_bioconductor-rhdf5_bioconductor-singlecellexperiment_r-seurat:a0f51df063bb9b2a'
    }
    withName: 'STAR_GENOMEGENERATE' {
        container = 'community.wave.seqera.io/library/htslib_samtools_star_gawk:ae438e9a604351a4'
    }
}
```

**2.** `MULTIQC` turns out to be a single tool, MultiQC 1.34, so look it up and replace it as
before

```bash
shelley find multiqc -v
```

**3.** Leave `ANNDATAR_CONVERT` and `STAR_GENOMEGENERATE` as they are

Their names list several tools each (for example `htslib`, `samtools`, `star`, and `gawk`).
These are mulled containers, which bundle multiple tools into one image. The process relies on
all of them together, and CVMFS only has single-tool containers, so there is no drop-in
replacement.

That's every container covered. Four of the eight processes (`FASTQC`, `GTF_GENE_FILTER`,
`STAR_ALIGN`, and `MULTIQC`) will now run straight from CVMFS. The other four share three
containers that still need downloading: the two mulled containers and the ScanPy container.

## 5. Set resource limits and the container cache {#resources}

Before running the pipeline, add two more settings to `bioshell.config`: limits on how much
each process can request, and a location for any containers that still need downloading.

```groovy
process {
    resourceLimits = [ cpus: 3, memory: '14.GB' ]
    // ... withName blocks ...
}

singularity {
    cacheDir = '/mnt/data/.singularity_cache'
}
```

Your BioShell has a fixed number of CPUs and amount of memory, and some pipeline processes
request more than that by default. `resourceLimits` caps what any single process can ask for.
The instance in this example has 4 CPUs and 16 GB of memory, so the limits leave 1 CPU and
2 GB free for Nextflow itself to keep running. Scale these values to your
[flavour](flavours).

`cacheDir` puts any containers that do need downloading on your data volume, rather than the
root disk.

In this example, the three containers pulled from Seqera took about 1.2 GB in total. Some
containers are much larger, around 2–5 GB each. Every container you can take from CVMFS
instead is space kept free for your own data and pipeline outputs.

## 6. Run the pipeline {#run}

With `bioshell.config` complete, you're ready for a real run.

**1.** Check that your finished `bioshell.config` looks like this

```groovy
process {
    resourceLimits = [ cpus: 3, memory: '14.GB' ]
    withName: 'FASTQC' {
        container = 'file:///cvmfs/singularity.galaxyproject.org/all/fastqc:0.12.1--hdfd78af_0'
    }
    withName: 'GTF_GENE_FILTER' {
        container = 'file:///cvmfs/singularity.galaxyproject.org/all/python:3.9'
    }
    withName: 'STAR_GENOMEGENERATE' {
        container = 'https://community-cr-prod.seqera.io/docker/registry/v2/blobs/sha256/26/268b4c9c6cbf8fa6606c9b7fd4fafce18bf2c931d1a809a0ce51b105ec06c89d/data'
    }
    withName: 'STAR_ALIGN' {
        container = 'file:///cvmfs/singularity.galaxyproject.org/all/star:2.7.10b--h9ee0642_0'
    }
    withName: 'ANNDATAR_CONVERT' {
        container = 'https://community-cr-prod.seqera.io/docker/registry/v2/blobs/sha256/d8/d8036a262b63ab783572e3cb3120b6b138593e19d93a1059b9bfef80785c7218/data'
    }
    withName: 'MULTIQC' {
        container = 'file:///cvmfs/singularity.galaxyproject.org/all/multiqc:1.34--pyhdfd78af_0'
    }
}

singularity {
    cacheDir = '/mnt/data/.singularity_cache'
}
```

**2.** In `run.sh`, change `inspect` back to `run`, and replace the last line with
`-c bioshell.config`

```bash
#!/bin/bash

nextflow run scrnaseq \
  --input samplesheet.csv \
  --outdir results \
  --aligner star \
  --protocol 10XV2 \
  --fasta data/GRCm38.p6.genome.chr19.fa \
  --gtf data/gencode.vM19.annotation.chr19.gtf \
  -profile singularity \
  -c bioshell.config
```

`-c` layers your config on top of the pipeline's own settings and the Singularity profile, so
you only need to include the settings you want to change.

{% include callout.html type="note" content="Nextflow always prioritises a config file passed with `-c` over the pipeline's built-in `nextflow.config`." %}

**3.** Run the script

```bash
bash run.sh
```

Nextflow prints the parameters from your run script, then starts working through the samples
in your samplesheet. This takes a few minutes.

As it runs, notice that processes using CVMFS containers, such as FastQC, start straight away
without pulling anything. Only the containers left as `https://` (and the pipeline's own ScanPy
containers) are downloaded, and they land in `/mnt/data/.singularity_cache`.

You'll also see new folders appear in `/mnt/data`:

- `work/` holds the logs and intermediate files for each task
- `results/` fills up with outputs as each process finishes, for example `fastqc/`, `star/`,
  and `multiqc/`
- `results/pipeline_info/` holds reports about the run itself, including how many resources
  each task used

Once all eight processes have completed, the pipeline reports that it finished successfully.
You have run nf-core/scrnaseq on BioShell with most of its containers taken straight from
CVMFS.

## Resuming failed runs {#resume}

If something goes wrong partway through, you don't need to start again from scratch. Fix the
cause (for example, a container entry in `bioshell.config`), then add `-resume` to the end of
the command in `run.sh`:

```bash
# truncated
  -c bioshell.config \
  -resume
```

Nextflow reuses the cached results of every task that already finished, and only reruns the
ones that failed or haven't run yet.

## Next steps: using your own pipeline {#checklist}

The same pattern works for any Nextflow pipeline, not just nf-core/scrnaseq. Your parameters,
processes, and registries will differ, but the steps stay the same:

**1.** Move your input data onto your data volume. For anything larger than a few files, use
[Globus](globus-howto) rather than `scp` or `wget`.

**2.** Work in your data volume and load the `nextflow` and `singularity` modules.

**3.** Clone the pipeline at a pinned release.

**4.** Run with `-preview` to list the processes your parameters will trigger.

**5.** Run `nextflow inspect -format config` to list their containers, and trim the output to
those processes.

**6.** Swap containers for CVMFS paths (`file:///cvmfs/...`) using `shelley find <tool> -v`.
Swap Galaxy Project containers directly, match Wave containers by tool and version, check
hash-only containers with `-profile docker`, and leave mulled containers as they are.

**7.** Add `resourceLimits` for your flavour and set `singularity.cacheDir` on your data volume.

**8.** Run the pipeline with `-profile singularity -c bioshell.config`, and use `-resume` if
you need to rerun.

## Acknowledgements {#acknowledgements}

We acknowledge the use of [Rosetta](https://sydney-informatics-hub.github.io/gpu-cluster-onboarding-guide/notebooks/transcription_workspace.html) for audio transcription of the accompanying video, conducted on the University of Sydney's Apollo GPU cluster.
