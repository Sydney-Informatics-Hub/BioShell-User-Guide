---
title: Using Nextflow with CVMFS
type: Using BioShell
description: How to run Nextflow workflows on BioShell using containers already available in CVMFS instead of downloading them.
---

If you are running a Nextflow workflow, you can direct processes to use containers already
in CVMFS rather than downloading them. This saves both time and storage.

Nextflow runs each workflow process in its own environment. When using containers, it normally
pulls the required image automatically. On BioShell, you can override this behaviour by
creating a config file that points processes to the existing CVMFS images.

{% include callout.html type="note" content="Nextflow always prioritises a `-config` file passed on the command line over the workflow's built-in `nextflow.config`." %}


## Using containers directly from CVMFS {#cvmfs-containers}

Create a config file (e.g. `cvmfs_path.config`) that points a process to the container
stored in CVMFS:

```groovy
process {
    withName: 'FASTQC' {
        container = 'file:///cvmfs/singularity.galaxyproject.org/all/fastqc:0.12.1--hdfd78af_0'
    }
}
```


## Using an installed SHPC module {#shpc-module}

Alternatively, if you have already installed a tool via Bio-Shelley or SHPC, you can
reference it by module name instead of a container path:

```groovy
process {
    withName: 'FASTQC' {
        module = 'fastqc/0.11.9'
    }
}
```

{% include callout.html type="note" content="The `module` value must match exactly what appears in `module avail`." %}


## Running with your config override {#run}

Pass your config file on the command line when running the workflow:

```bash
nextflow run main.nf -profile singularity -config cvmfs_path.config
```
