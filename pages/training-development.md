---
title: Training development instructions
description: How to set up and prepare a BioShell training environment for the BioCommons Training Cooperative, including VM configuration, CVMFS use, trainee directory layout, and pre-snapshot requirements.
---

{% include callout.html type="warning" content="These instructions are a work in progress. If anything is unclear, reach out to the training team directly. We appreciate your support in making these as simple to follow as possible, and feedback is very welcome." %}

This guide is for **training developers** who are setting up BioShell training environments for the BioCommons Training Cooperative. You will configure tools, build training materials, and prepare a template that gets automatically applied to trainee accounts when VMs are provisioned. 

{% include callout.html type="important" content="**New to this? Key terms used throughout this guide:**<br><br>**VM (virtual machine)** — a virtual computer provisioned in the cloud that you log into and configure. Each training developer gets a dev VM, and each trainee gets their own VM built from it.<br><br>**Snapshot** — a saved image of a VM's disk at a point in time. Once your dev VM is set up exactly how you want it, it is *snapshotted* so that identical trainee VMs can be built from that image.<br><br>**Template (`/etc/skel/`)** — the skeleton directory whose contents are copied into every new user's home directory when their account is created. Whatever you place in `/etc/skel/` is exactly what trainees see on first login.<br><br>**Provisioning script** — the script that runs automatically when a trainee VM is built. It creates the `userNN` account, sets a password, and populates the home directory from `/etc/skel/`.<br><br>**CVMFS** — a read-only, on-demand network file system providing bioinformatics tools and reference data. Content used from CVMFS does not count against your VM's disk budget.<br><br>**Symlink** — a pointer to a file stored elsewhere (for example on CVMFS), used instead of copying large files into the trainee directory.<br><br>**shelley** — a helper for finding and installing containerised tools from CVMFS as loadable modules.<br><br>**cloud-init** — the service that configures a VM on first boot (hostname, network, datasource). It must be checked before snapshotting." %}

## How it works {#how-it-works}

At a high level, preparing a training environment follows five steps:

1. **Get a dev VM** and log in (see [VM setup and access](#vm-setup)).
2. **Build and test** your training materials in your home directory, using CVMFS tools and references wherever possible (see [Developing training materials](#developing-materials)).
3. **Assemble the template** by copying your home directory into `/etc/skel/`, so every trainee starts with an identical setup (see [Assemble the template](#assemble-template)).
4. **Complete the pre-snapshot checks** so trainee VMs build correctly (see [Pre-snapshot requirements](#pre-snapshot)).
5. **Request a snapshot.** Trainee VMs are then built from that snapshot.

When a trainee VM is built from your snapshot, the provisioning script runs automatically and:

- creates the trainee's account, with username `userNN` (for example `user1`)
- sets a password that is derived deterministically from the username and is unique per VM
- copies the contents of `/etc/skel/` (as it was at snapshot time) into the trainee's home directory
- clears `/etc/skel/` afterwards to reduce the VM image size


## 1. VM setup and access {#vm-setup}

### Launch a VM instance

VM instances are provisioned by the training VM manager. Each dev machine is named with the prefix `D` followed by a number (for example `D1`).


### Log in via SSH

The training team will provide you with a username and IP address. Your username follows the format `tdevNN`, where `NN` matches your VM's prefix number (for example, on VM `D1` your username is `tdev1`).

```bash
ssh tdevNN@<IP_Address>
```

### Accessing RStudio and JupyterLab {#ports}

RStudio and JupyterLab run as web services on your VM and are accessible directly in your browser. See [Interactive environments](interactive) for full details on both environments.

Once you have an active SSH connection, open your browser and go to:

- **JupyterLab:** `http://<IP_Address>:8888`
- **RStudio:** `http://<IP_Address>:8787`

Replace `<IP_Address>` with the IP address provided by the training team.


## 2. Developing training materials {#developing-materials}

Set up your home directory (`/home/tdevNN`) exactly as you want trainees to experience it. Build and test your workflows there, using CVMFS containers and references throughout. The goal is a working, self-contained environment that a trainee could follow from start to finish.

{% include callout.html type="tip" content="**Keep data small:** Small training datasets are best practice regardless of VM size — they download and run quickly, so trainees spend their time learning rather than waiting, and the workflow fits comfortably within a workshop session. Use the minimum input data needed to demonstrate the workflow. When the Australian BioCommons dataset repository is ready, it will be served over CVMFS (like the Galaxy reference repositories), so data accessed from it will not count against VM size. Until then, if your ideal dataset is too large to copy into every trainee home directory, see [If resources are not available on CVMFS](#non-cvmfs) for how to pull it at boot time instead." %}

Once everything works end-to-end, your home directory becomes the template.


### Disk budget {#disk-budget}

The base VM template takes up approximately **13 GB** on a **30 GB** disk, leaving roughly **16 GB** of usable space. That space has to cover:

- Training materials saved in the template directory (`/etc/skel/`)
- CVMFS-cached container layers and reference data (written to the CVMFS cache on first use)
- Trainee working files generated during the session

{% include callout.html type="tip" content="**Watch the disk:** If the disk fills up during user creation, the `useradd` step will fail and the trainee account will not be created. Keep `/etc/skel/` as lean as possible." %}

Check how much space you have with:

```bash
df -h
```

Example output:

```
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           197M  1.1M  196M   1% /run
/dev/vda2        30G   14G   17G  43% /
tmpfs           984M     0  984M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           197M   16K  197M   1% /run/user/1000
```

If you need to recover some disk space, these commands can help:

```bash
sudo apt clean                          # Clear apt cache (~640 MB saving)
sudo rm -rf /root/.cache/go-build       # Clear Go build cache (~490 MB)
sudo rm -rf /tmp/* /var/tmp/*           # Clear temp files
sudo journalctl --vacuum-size=50M       # Trim system logs
sudo cvmfs_config wipecache             # CVMFS will automatically refill it with the files as needed
```

### Using CVMFS resources {#cvmfs}

CVMFS gives you read-only, on-demand access to reference data and Singularity containers. Because it does not count against your disk budget, always prefer CVMFS resources over local copies wherever possible.

For a full walkthrough of what is available and how to use it, see the [CVMFS and reference data guide](tools).

#### Finding and installing tools with shelley

Use `shelley` to search for and install tools from CVMFS-hosted Singularity images:

```bash
# Search for available tools
shelley find <toolname>

# Build a module-loadable tool via its CVMFS container
shelley build <toolname>
```

#### Symlinking reference data from CVMFS {#symlinking}

Rather than copying reference files into the trainee directory, create a symlink that points directly to the CVMFS path. For example:

```bash
ln -s /cvmfs/data.galaxyproject.org/byhand/CHM13_T2T_v2.0/seq/CHM13_T2T_v2.0.fa CHM13_T2T_v2.0.fa
```

This creates a symlink in your current directory pointing to the CVMFS source. You can also give it a custom name or place it somewhere else:

```bash
ln -s /cvmfs/data.galaxyproject.org/byhand/CHM13_T2T_v2.0/seq/CHM13_T2T_v2.0.fa /path/to/destination/my_reference.fa
```

{% include callout.html type="note" content="**Symlinks must use relative paths or CVMFS paths.** If you create a symlink pointing to an absolute path inside your home directory (for example `/home/tdev01/data/reference.fa`), that path will not exist on trainee VMs and the link will be broken. Always point symlinks either at a CVMFS path (absolute paths under `/cvmfs/` are safe because CVMFS is available on all VMs) or at a relative path within the directory structure that will be copied to `/etc/skel/`." %}


### RStudio and JupyterLab environments {#interactive-envs}

If your training module uses RStudio or JupyterLab, any R packages, Python packages, or Jupyter kernels you install are written to your home directory, so they follow the same `/etc/skel/` workflow as everything else. Save notebooks in the state you want trainees to open them in — for example, clear all cell outputs so each notebook starts fresh.

#### R packages

Install packages from within R as normal:

```r
install.packages("ggplot2")
BiocManager::install("DESeq2")
```

R writes packages to `~/R/x86_64-pc-linux-gnu-library/<version>/` by default. This directory will be picked up automatically when you copy your home directory to `/etc/skel/`.

{% include callout.html type="tip" content="**Watch the size:** R packages for workflows like scRNA-seq (Seurat, Bioconductor suites) can be several gigabytes." %}

Check the size before copying:

```bash
du -sh ~/R/
```

#### Python packages and Jupyter kernels

Install packages to your user directory:

```bash
pip install --user <package>
```

Packages land in `~/.local/lib/python<version>/site-packages/`. If you register a custom Jupyter kernel (for example a conda environment), its kernel spec is written to `~/.local/share/jupyter/kernels/`. Both locations are included when you copy your home directory to `/etc/skel/`.

```bash
du -sh ~/.local/
```

### If resources are not available on CVMFS {#non-cvmfs}

If a required container or reference dataset is not on CVMFS and is too large to include directly in the trainee directory, it needs to be fetched at boot time via the VM provisioning script. Prepare the required scripts and share them with the training VM manager.


#### Adding a pull step to the provisioning script

Downloads should be added before the `useradd` block, targeting the relevant folder inside `/etc/skel/`:

```bash
# Pull a Singularity container into the trainee containers directory
mkdir -p /home/$USERNAME/containers
singularity pull /home/$USERNAME/containers/<tool>.sif <registry-path>

# Pull a reference file into the trainee references directory
mkdir -p /home/$USERNAME/references
wget -q -O /home/$USERNAME/references/<file> <url>
```

Resources pulled this way will appear in the correct location in the trainee's home directory, matching the layout described in [Trainee directory layout](#directory-layout).

{% include callout.html type="note" content="Boot-time pulls count toward your disk budget. Factor their size into your 30 GB total." %}


## 3. Assemble the template {#assemble-template}

With your workflow tested and working, package your home directory into the template (`/etc/skel/`) that every trainee VM is built from.

### Trainee directory layout {#directory-layout}

Organise your home directory to reflect what you want trainees to see when they first log in. For example:

```
~/
├── README.md                  # Introduction and step-by-step instructions
├── data/                      # Minimal test input data if not available on CVMFS otherwise symlinked 
├── containers/                # Containers symlinked unless not available on CVMFS (see above)
├── references/                # References symlinked unless not available on CVMFS (see above)
├── configs/                   # Workflow configuration files
├── scripts/                   # Example and helper scripts
└── results/                   # Empty output directory for trainee use
```

Not every module will need all of these folders. Only include what is relevant. The next section covers copying this directory into the template.

### Copy your directory into the template

Before copying, check how large your home directory is:

```bash
du -sh /home/<username>/
```

Once you are happy with the contents, copy everything to `/etc/skel/`:

```bash
sudo cp -r /home/<username>/. /etc/skel/
```

Replace `<username>` with your actual username (for example `tdev01`).

Then review what landed in the template and remove anything you do not want:

```bash
ls -a /etc/skel/
```

The contents of `/etc/skel/` will be copied automatically into every trainee's home directory when their account is created.

Once you are confident the template is correct, you can clean up your own home directory to free space:

```bash
rm -rf ~/*
```

{% include callout.html type="tip" content="Ask the training VM manager to take a snapshot at key checkpoints if you want a backup before making significant changes." %}


## 4. Pre-snapshot requirements {#pre-snapshot}


### Cloud-init datasource check {#cloud-init}

Running `apt upgrade` during VM setup can silently change the cloud-init configuration, which causes all VMs built from your snapshot to inherit the wrong hostname. Check for this before requesting a snapshot:

```bash
ls /etc/cloud/cloud.cfg.d/
```

A working VM contains only `05_logging.cfg` and `README`. If that is all you see, no action is needed — continue to the [pre-snapshot checklist](#checklist).

If `90_dpkg.cfg` is also present, `apt` has modified the cloud-init configuration and you must repair it using the steps below before snapshotting.

<details markdown="1" id="cloud-init-fix">
<summary>Fix: <code>apt</code> has changed the cloud-init configuration</summary>

**1. Remove the offending apt file**

```bash
sudo rm /etc/cloud/cloud.cfg.d/90_dpkg.cfg
```

**2. Verify the datasource order**

```bash
cat /etc/cloud/cloud.cfg | grep -A5 "datasource_list"
```

The output should list only `ConfigDrive` and `OpenStack`:

```yaml
datasource_list:
  - ConfigDrive
  - OpenStack
```

**3. Clean cloud-init state**

`cloud-init clean` deletes `/etc/cloud/ds-identify.cfg`, so you will need to recreate it afterwards.

```bash
sudo cloud-init clean --logs
sudo tee /etc/cloud/ds-identify.cfg << 'EOF'
policy: enabled
EOF
cat /etc/cloud/ds-identify.cfg
```

The final command should print `policy: enabled`.

**4. Confirm the datasource**

```bash
sudo cloud-init init
sudo cloud-init status --long
```

Look at the `detail` line in the output. It should read `DataSourceOpenStackLocal` or `DataSourceOpenStack`:

```
detail: DataSourceOpenStackLocal [net,ver=2]
```

If it reads `DataSourceNoCloud`, work through these steps again. If it still does not resolve, contact the training team.

</details>

### Pre-snapshot checklist {#checklist}

Run through this before taking any snapshot:

- [ ] Workflow tested end-to-end from a trainee's perspective
- [ ] All tools use CVMFS Singularity containers installed via `shelley` where available
- [ ] All references point to CVMFS paths or symlinks where available
- [ ] Input data is as small as practical
- [ ] `du -sh /home/<username>/` confirms the directory size is acceptable
- [ ] Any non-CVMFS resources that are too large to bundle have been prepared as provisioning script pull steps and shared with the training VM manager
- [ ] `sudo cp -r /home/<username>/. /etc/skel/` completed successfully
- [ ] `/etc/skel/` contents verified before snapshotting
- [ ] All symlinks in `/etc/skel/` point to CVMFS paths or relative paths (no absolute paths into `/home/tdevNN/`)
- [ ] Total estimated disk usage (base ~14 GB + skel + boot-time pulls) is comfortably under 30 GB
- [ ] `/etc/cloud/ds-identify.cfg` is present and reads `policy: enabled`
