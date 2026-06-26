---
title: Training development instructions
description: How to set up and prepare a BioShell training environment for the BioCommons Training Cooperative, including VM configuration, CVMFS use, trainee directory layout, and pre-snapshot requirements.
---

> **Draft:** These instructions are under active development. Contact Mitchell for
> clarification on any steps or requirements — feedback is welcome.

This guide is for **training developers** (`tdevNN` users) setting up BioShell training
environments on virtual machines. You will configure tools, build training materials, and
prepare a template that is automatically applied to trainee accounts when VMs are provisioned.

---

## 1. VM setup and access {#vm-setup}

### Launch a VM instance

VM instances are provisioned by Giorgia or Mitchell. Machines are named with the prefix `D`
(for dev) followed by a number, for example `D1`.

### Log in via SSH

A username and IP address will be provided by the training team. Your username follows the
format `tdevNN` where `NN` matches your VM's prefix number.

```bash
ssh tdevNN@<IP_Address>
```

---

## 2. Disk budget {#disk-budget}

The base VM template is approximately **12 GB** on a **30 GB** disk, leaving roughly **16 GB**
of usable space. This must cover:

- Training materials saved in the template directory (`/etc/skel/`)
- CVMFS-cached container layers and reference data (written to the CVMFS cache on first use)
- Trainee working files generated during the session

> **Critical:** If the disk fills up during user creation, the `useradd` step will fail and
> the trainee account will not be created. Keep `/etc/skel/` as lean as possible.

Check disk availability with:

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

To recover disk space:

```bash
sudo apt clean                          # Clear apt cache (~640 MB saving)
sudo rm -rf /root/.cache/go-build       # Clear Go build cache (~490 MB)
sudo rm -rf /tmp/* /var/tmp/*           # Clear temp files
sudo journalctl --vacuum-size=50M       # Trim system logs
```

---

## 3. Using CVMFS resources {#cvmfs}

CVMFS provides read-only, on-demand access to reference data and Singularity containers while
consuming very little cached local disk space. Always prioritise CVMFS resources over local
copies — they do not count against the disk budget.

For documentation on using data in the CVMFS repository, see the
[CVMFS reference data guide](https://docs.google.com/document/d/11YKFZjxzpjKPvyqK3SuAX7zUdgB24ziuGtgdvPcYrBs/edit?tab=t.0#heading=h.fbpu5xkpq96d).

### Singularity containers via shelley-bio

Use `shelley-bio` to find and install tools from CVMFS-hosted Singularity images:

```bash
# Search for available tools
shelley-bio find <toolname>

# List available versions
shelley-bio versions <toolname>

# Build a module-loadable tool via its CVMFS container
shelley-bio build <toolname>
```

Always check whether a required tool or reference is available on CVMFS before considering
alternatives. Where a CVMFS path exists, point configs and scripts directly at it — do not
copy resources into the trainee directory.

---

## 4. Developing training materials {#developing-materials}

Set up your home directory (`/home/tdevNN`) exactly as you want trainees to experience it.
Build and test your workflows there, using CVMFS containers and references throughout. The
goal is a working, self-contained training environment that a trainee could follow from
start to finish.

> **Data size:** When the Australian BioCommons dataset repository is ready it will be the
> main access point for training data — like CVMFS, data stored there will not count against
> VM size. Until then, keep data small. Use the minimum input data needed to demonstrate the
> workflow. If your ideal dataset is too large to copy to every trainee home directory, see
> [Section 6](#non-cvmfs) for how to pull it at boot time instead.

Once everything works end-to-end, your home directory becomes the template.

---

## 5. Trainee directory layout {#directory-layout}

Organise your home directory to reflect what trainees should see on login:

```
~/
├── README.md                  # Introduction and step-by-step instructions
├── data/                      # Minimal test input data if not available on CVMFS
├── containers/                # Containers not available on CVMFS (see Section 6)
├── references/                # References not available on CVMFS (see Section 6)
├── configs/                   # Workflow configuration files
├── scripts/                   # Example and helper scripts
└── results/                   # Empty output directory for trainee use
```

Not all directories are needed for every module — include only what is relevant.

### Check your size before copying

```bash
du -sh /home/<username>/
```

Once satisfied, copy your home directory to `/etc/skel/`:

```bash
sudo cp -r /home/<username>/. /etc/skel/
```

Replace `<username>` with your actual username (e.g. `tdev01`).

Review and clean up the template directory:

```bash
ls -a /etc/skel/
```

Remove any unwanted files. The contents of `/etc/skel/` act as the template and will be
applied automatically to every trainee's home directory when their account is created.

Once you are confident the template is correct, delete the contents of your own home
directory to free space.

> **Tip:** Ask to have a snapshot taken at key checkpoints if you want a backup before
> making significant changes.

---

## 6. If resources are not available on CVMFS {#non-cvmfs}

If a required container or reference is not available on CVMFS and is too large to include
directly in the trainee directory, it must be fetched at boot time via the VM provisioning
script. Prepare the required scripts and share with Giorgia or Mitchell.

### Adding a pull step to the provisioning script

Add downloads before the `useradd` block, targeting the relevant folder within `/etc/skel/`:

```bash
# Pull a Singularity container into the trainee containers directory
mkdir -p /home/$USERNAME/containers
singularity pull /home/$USERNAME/containers/<tool>.sif <registry-path>

# Pull a reference file into the trainee references directory
mkdir -p /home/$USERNAME/references
wget -q -O /home/$USERNAME/references/<file> <url>
```

Resources pulled this way will appear in the correct location within the trainee's home
directory, consistent with the layout in [Section 5](#directory-layout).

> **Note:** Boot-time pulls add to your disk budget. Factor their size into your 30 GB total.

---

## 7. How trainees are provisioned {#provisioning}

When a VM is built from your snapshot (done by Giorgia or Mitchell), the provisioning script
runs automatically:

- The trainee's username will be `userNN` (e.g. `user1`)
- Their password is deterministically generated from their username, unique per VM
- The contents of `/etc/skel/` at snapshot time become the trainee's home directory
- After the user is created, `/etc/skel/` is cleared to reduce VM image size

---

## 8. Cloud-init datasource check (pre-snapshot requirement) {#cloud-init}

Running `apt upgrade` during VM setup can silently alter cloud-init configuration, causing all
VMs built from the snapshot to inherit the wrong hostname. The following must be checked and
corrected before every snapshot.

### Step 1 — Check for the offending apt file

```bash
ls /etc/cloud/cloud.cfg.d/
```

A working VM should contain only `05_logging.cfg` and `README`. If `90_dpkg.cfg` is present,
remove it:

```bash
sudo rm /etc/cloud/cloud.cfg.d/90_dpkg.cfg
```

### Step 2 — Verify the datasource order

```bash
cat /etc/cloud/cloud.cfg | grep -A5 "datasource_list"
```

The output should show only `ConfigDrive` and `OpenStack`:

```yaml
datasource_list:
  - ConfigDrive
  - OpenStack
```

### Step 3 — Clean cloud-init state

```bash
sudo cloud-init clean --logs
```

> **Important:** `cloud-init clean` deletes `/etc/cloud/ds-identify.cfg`. Recreate it
> immediately:

```bash
sudo tee /etc/cloud/ds-identify.cfg << 'EOF'
policy: enabled
EOF
```

Verify it was created correctly:

```bash
cat /etc/cloud/ds-identify.cfg
```

Expected output: `policy: enabled`

### Step 4 — Confirm the datasource

```bash
sudo cloud-init init
sudo cloud-init status --long
```

Check the `detail` line in the output — it should read `DataSourceOpenStackLocal` or
`DataSourceOpenStack`:

```
detail: DataSourceOpenStackLocal [net,ver=2]
```

If it reads `DataSourceNoCloud`, recheck Steps 1–4 and contact Mitchell or Giorgia if the
problem persists.

---

## 9. Pre-snapshot checklist {#checklist}

- [ ] Workflow tested end-to-end from a trainee's perspective
- [ ] All tools use CVMFS Singularity containers via `shelley-bio` where available
- [ ] All references point to CVMFS paths where available
- [ ] Input data is as small as practical
- [ ] `du -sh /home/<username>/` confirms the directory size is acceptable
- [ ] Any non-CVMFS resources that are too large to bundle are pulled via the provisioning script
- [ ] `sudo cp -r /home/<username>/. /etc/skel/` completed successfully
- [ ] `/etc/skel/` contents verified before snapshotting
- [ ] Total estimated disk usage (base ~12 GB + skel + boot-time pulls) comfortably under 30 GB
- [ ] `/etc/cloud/ds-identify.cfg` is present and reads `policy: enabled`
