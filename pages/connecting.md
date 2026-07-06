---
title: Connecting to BioShell
type: Getting started
description: How to connect to your BioShell environment via SSH and what to expect on first login.
---

Once your environment is provisioned you will receive connection details by email. BioShell
uses SSH key authentication — you will need the key pair you generated and submitted during
your access request.

> **Important:** Your connection details are provided as part of your provisioning
> notification. Keep them secure and do not share them publicly.

If you have not yet generated and submitted your SSH key, see the
[SSH key generation guide](ssh-keys) first.

---

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

[AUTHOR TO SUPPLY — confirm username format]

> **Tip:** If you are connecting from outside your institution's network you may need a VPN
> or SSH tunnel. Contact your local IT support if you are unsure.

---

## Interactive environments {#interactive-envs}

Once connected, you can also open browser-based environments directly:

| Environment | URL |
|-------------|-----|
| JupyterLab | `http://<your-bioshell-ip>:8888` |
| RStudio | `http://<your-bioshell-ip>:8787` |

See the [Interactive environments](interactive) page for full setup instructions.

---

## First login {#first-login}

[AUTHOR TO SUPPLY — describe what users see on first login: the welcome banner, any
environment setup steps, and how to verify the environment is working correctly.]

### CVMFS access {#cvmfs-access}

[AUTHOR TO SUPPLY — explain how CVMFS appears on first login. Cover: running
`cvmfs_config probe` to verify mounts are active, which repositories are mounted, and
any first-access latency users should expect. Link to the Tools and reference data page
for full software installation instructions.]

### Storage volumes {#storage}

[AUTHOR TO SUPPLY — describe the storage volumes available to users: home directory,
scratch space, shared project space. Include: default quota sizes, where to store
long-term vs. temporary files, and any policies on data retention or deletion.]
