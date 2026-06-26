---
title: Training development instructions
description: How to set up and prepare a BioShell training environment for the BioCommons Training Cooperative, including VM configuration, CVMFS use, trainee directory layout, and pre-snapshot requirements.
---

> **Draft:** These instructions are a work in progress. If anything is unclear, reach out to Mitchell directly. Feedback is very welcome.

This guide is for **training developers** (`tdevNN` users) who are setting up BioShell training environments for the BioCommons Training Cooperative. You will configure tools, build training materials, and prepare a template that gets automatically applied to trainee accounts when VMs are provisioned.

---

## 1. VM setup and access {#vm-setup}

### Launch a VM instance

VM instances are provisioned by Giorgia or Mitchell. Each dev machine is named with the prefix `D` followed by a number (for example `D1`).

### Log in via SSH

The training team will provide you with a username and IP address. Your username follows the format `tdevNN`, where `NN` matches your VM's prefix number.

```bash
ssh tdevNN@<IP_Address>
```

---

## 2. Disk budget {#disk-budget}

The base VM template takes up approximately **12 GB** on a **30 GB** disk, leaving roughly **16 GB** of usable space. That space has to cover:

- Training materials saved in the template directory (`/etc/skel/`)
- CVMFS-cached container layers and reference data (written to the CVMFS cache on first use)
- Trainee working files generated during the session

> **Watch the disk:** If the disk fills up during user creation, the `useradd` step will fail and the trainee account will not be created. Keep `/etc/skel/` as lean as possible.

Check how much space you have with:

```bash
df -h
```

Example output:

```
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           197M  1.1M  196M   1% /run
/dev/vda2        30G   12G   17G  43% /
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
```

---

## 3. Using CVMFS resources {#cvmfs}

CVMFS gives you read-only, on-demand access to reference data and Singularity containers. Importantly, CVMFS does not count against your disk budget, so you should always prefer CVMFS resources over local copies wherever possible.

For a full walkthrough of what is available and how to use it, see the [CVMFS and reference data guide](tools).

### Finding and installing tools with shelley-bio

Use `shelley-bio` to search for and install tools from CVMFS-hosted Singularity images:

```bash
# Search for available tools
shelley-bio find <toolname>

# List available versions
shelley-bio versions <toolname>

# Build a module-loadable tool via its CVMFS container
shelley-bio build <toolname>
```

### Symlinking reference data from CVMFS {#symlinking}

Rather than copying reference files into the trainee directory, create a symlink that points directly to the CVMFS path. For example:

```bash
ln -s /cvmfs/data.galaxyproject.org/byhand/CHM13_T2T_v2.0/seq/CHM13_T2T_v2.0.fa CHM13_T2T_v2.0.fa
```

This creates a symlink in your current directory pointing to the CVMFS source. You can also give it a custom name or place it somewhere else:

```bash
ln -s /cvmfs/data.galaxyproject.org/byhand/CHM13_T2T_v2.0/seq/CHM13_T2T_v2.0.fa /path/to/destination/my_reference.fa
```

---

## 4. Developing training materials {#developing-materials}

Set up your home directory (`/home/tdevNN`) exactly as you want trainees to experience it. Build and test your workflows there, using CVMFS containers and references throughout. The goal is a working, self-contained environment that a trainee could follow from start to finish.

> **Keep data small:** When the Australian BioCommons dataset repository is ready, it will be the main access point for training data. Like CVMFS, data stored there will not count against VM size. Until then, use the minimum input data needed to demonstrate the workflow. If your ideal dataset is too large to copy to every trainee home directory, see [Section 6](#non-cvmfs) for how to pull it at boot time instead.

Once everything works end-to-end, your home directory becomes the template.

---

## 5. Trainee directory layout {#directory-layout}

Organise your home directory to reflect what trainees should see when they first log in. A typical layout might look like this:

```
~/
├── README.md                  # Introduction and step-by-step instructions
├── data/                      # Minimal test input data if not available on CVMFS otherwise symlinked 
├── containers/                # Containers symlinked unless not available on CVMFS (see Section 6)
├── references/                # References symlinked unless not available on CVMFS (see Section 6)
├── configs/                   # Workflow configuration files
├── scripts/                   # Example and helper scripts
└── results/                   # Empty output directory for trainee use
```

Not every module will need all of these folders. Only include what is relevant.

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

> **Tip:** Ask Giorgia or Mitchell to take a snapshot at key checkpoints if you want a backup before making significant changes.

---

## 6. If resources are not available on CVMFS {#non-cvmfs}

If a required container or reference dataset is not on CVMFS and is too large to include directly in the trainee directory, it needs to be fetched at boot time via the VM provisioning script. Prepare the required scripts and share them with Giorgia or Mitchell.

### Adding a pull step to the provisioning script

Downloads should be added before the `useradd` block, targeting the relevant folder inside `/etc/skel/`:

```bash
# Pull a Singularity container into the trainee containers directory
mkdir -p /home/$USERNAME/containers
singularity pull /home/$USERNAME/containers/<tool>.sif <registry-path>

# Pull a reference file into the trainee references directory
mkdir -p /home/$USERNAME/references
wget -q -O /home/$USERNAME/references/<file> <url>
```

Resources pulled this way will appear in the correct location in the trainee's home directory, matching the layout described in [Section 5](#directory-layout).

> **Note:** Boot-time pulls count toward your disk budget. Factor their size into your 30 GB total.

---

## 7. How trainees are provisioned {#provisioning}

When a VM is built from your snapshot (done by Giorgia or Mitchell), the provisioning script runs automatically:

- The trainee's username will be `userNN` (for example `user1`)
- Their password is generated deterministically from their username, and is unique per VM
- The contents of `/etc/skel/` at snapshot time become the trainee's home directory
- After the user is created, `/etc/skel/` is cleared to reduce VM image size

---

## 8. Cloud-init datasource check (required before every snapshot) {#cloud-init}

Running `apt upgrade` during VM setup can silently change cloud-init configuration, which causes all VMs built from your snapshot to inherit the wrong hostname. Please check and fix this before asking the VM to be snapshot.

### Step 1: Check for the offending apt file

```bash
ls /etc/cloud/cloud.cfg.d/
```

A working VM should only contain `05_logging.cfg` and `README`. If `90_dpkg.cfg` is present, remove it:

```bash
sudo rm /etc/cloud/cloud.cfg.d/90_dpkg.cfg
```

### Step 2: Verify the datasource order

```bash
cat /etc/cloud/cloud.cfg | grep -A5 "datasource_list"
```

The output should list only `ConfigDrive` and `OpenStack`:

```yaml
datasource_list:
  - ConfigDrive
  - OpenStack
```

### Step 3: Clean cloud-init state

```bash
sudo cloud-init clean --logs
```

> **Important:** `cloud-init clean` deletes `/etc/cloud/ds-identify.cfg`. You will need to recreate it:

```bash
sudo tee /etc/cloud/ds-identify.cfg << 'EOF'
policy: enabled
EOF
```

Verify it looks right:

```bash
cat /etc/cloud/ds-identify.cfg
```

Expected output: `policy: enabled`

### Step 4: Confirm the datasource

```bash
sudo cloud-init init
sudo cloud-init status --long
```

Look at the `detail` line in the output. It should read `DataSourceOpenStackLocal` or `DataSourceOpenStack`:

```
detail: DataSourceOpenStackLocal [net,ver=2]
```

If it reads `DataSourceNoCloud`, work through Steps 1 to 4 again. If it still does not resolve, contact Mitchell or Giorgia.

---

## Pre-snapshot checklist {#checklist}

Run through this before taking any snapshot:

- [ ] Workflow tested end-to-end from a trainee's perspective
- [ ] All tools use CVMFS Singularity containers installed via `shelley-bio` where available
- [ ] All references point to CVMFS paths or symlinks where available
- [ ] Input data is as small as practical
- [ ] `du -sh /home/<username>/` confirms the directory size is acceptable
- [ ] Any non-CVMFS resources that are too large to bundle have been prepared as provisioning script pull steps and shared with Giorgia or Mitchell
- [ ] `sudo cp -r /home/<username>/. /etc/skel/` completed successfully
- [ ] `/etc/skel/` contents verified before snapshotting
- [ ] Total estimated disk usage (base ~12 GB + skel + boot-time pulls) is comfortably under 30 GB
- [ ] `/etc/cloud/ds-identify.cfg` is present and reads `policy: enabled`
