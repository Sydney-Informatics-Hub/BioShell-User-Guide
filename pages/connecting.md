---
title: Connecting to BioShell
type: Using BioShell
description: How to connect to your BioShell environment via SSH and what to expect on first login.
---

Once your environment is provisioned you will receive connection details by email. BioShell
uses SSH key authentication, you will need the key pair you generated and submitted during
your access request.

{% include callout.html type="important" content="Your connection details are provided as part of your provisioning notification. Keep them secure and do not share them publicly." %}

If you have not yet generated and submitted your SSH key, see the
[**SSH key generation guide**](ssh-keys) first.


## Connect via SSH {#ssh-connect}

Add an entry to `~/.ssh/config` so SSH automatically uses your BioShell key without needing
to specify it each time:

```text
Host bioshell
    HostName <your-bioshell-ip>
    User <username>
    IdentityFile ~/.ssh/bioshell_key
```

Then connect with:

```bash
ssh bioshell
```

Or connect directly without the config entry:

```bash
ssh -i ~/.ssh/bioshell_key <username>@<your-bioshell-ip>
```

## First login {#first-login}

When you connect for the first time you will land in your home directory. Confirm where you
are with `pwd`:

```bash
pwd
```

You should see your home directory path:

```text
/home/<username>
```

Once you are logged in, verify the two things you will rely on most: the software file
system (CVMFS) and your storage volumes.

### CVMFS access {#cvmfs-access}

BioShell delivers its bioinformatics tools and reference data through **CVMFS**, a read-only
network file system mounted at `/cvmfs/`. Confirm the repositories are connected by running
the probe command:

```bash
cvmfs_config probe
```

You should see `OK` for each repository:

```text
Probing /cvmfs/data.galaxyproject.org... OK
Probing /cvmfs/singularity.galaxyproject.org... OK
```

You can also list the mounted repositories directly:

```bash
ls /cvmfs/
```

```text
data.galaxyproject.org  singularity.galaxyproject.org
```

| Repository                      | Contents                                                    |
| ------------------------------- | ----------------------------------------------------------- |
| `singularity.galaxyproject.org` | Containerised bioinformatics tools from BioContainers       |
| `data.galaxyproject.org`        | Reference genome builds and pre-built indexes               |

{% include callout.html type="note" content="The first time you access a path in CVMFS it may take a moment while metadata is fetched and cached. Subsequent access is fast. If a repository shows `Failed!`, wait a moment and run `cvmfs_config probe` again." %}

For how to find, install, and load tools from these repositories, see the
[**Tools and reference data**](tools) page.

### Storage volumes {#storage}

BioShell provides two storage locations:

| Location | Volume | Purpose |
|----------|--------|---------|
| `/home/<username>` | Root volume (`/dev/vda2`) | Your home directory: configuration, scripts, and installed modules. |
| `/mnt/data` | Data volume (`/dev/vdb`) | Your working data: analysis inputs and outputs. |

Use `lsblk` to see how the volumes are laid out:

```bash
lsblk
```

```text
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS
vda    253:0    0  30G  0 disk
├─vda1 253:1    0   1M  0 part
└─vda2 253:2    0  30G  0 part /
vdb    253:16   0 100G  0 disk /mnt/data
```

This shows your home directory on the root volume (`vda`) and your data volume (`vdb`) as a
separate disk. Check the size and available space of your home directory with `df`:

```bash
df -hT /home/<username>/
```

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/vda2      ext4   30G   14G   15G  49% /
```

And your data volume:

```bash
df -hT /mnt/data
```

```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/vdb       ext4  100G   28K   99G   1% /mnt/data
```

{% include callout.html type="note" content="The sizes shown above are examples only. Your home and data volume capacities depend on the allocation provisioned for your project." %}

{% include callout.html type="tip" content="Store your analysis inputs and outputs on the data volume at `/mnt/data` rather than in your home directory. The data volume is provisioned for your working data and keeps it separate from your home directory and installed software." %}


## Interactive environments {#interactive-envs}

Once connected, you can also open browser-based environments directly:

| Environment | URL |
|-------------|-----|
| JupyterLab | `http://<your-bioshell-ip>:8888` |
| RStudio | `http://<your-bioshell-ip>:8787` |

See the [**Interactive environments**](interactive) page for full setup instructions.
