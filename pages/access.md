---
title: Accessing BioShell
description: How to check your eligibility, request access to BioShell, and connect to your
  environment for the first time.
---

## Eligibility and acceptable use {#eligibility}

BioShell is a national resource funded through NCRIS. Access is governed by the
[ABLeS programme](https://australianbiocommons.github.io/ables/index) (Australian BioCommons
Leadership Share).

Every access request must satisfy the five core ABLeS principles:

1. Your project must be life-science related and produce data, research, training, or software outcomes.
2. Your team must have sufficient expertise to responsibly use bioinformatics infrastructure.
3. Your resource usage must be planned and proportional — national infrastructure is finite.
4. Your project should support collaboration and sharing of outputs where appropriate.
5. You must comply with all relevant facility policies.

For full eligibility criteria, see the [ABLeS eligibility page](https://australianbiocommons.github.io/ables/eligibility).

### Who can apply?

| User type | Description |
|-----------|-------------|
| Research projects | Academic, government, or industry life-science research with defined outcomes |
| Development projects | Bioinformatics tool or workflow development intended for community use |
| Self-paced learning | Independent exploration and learning; no training event required |
| Training projects | Nationally delivered or institutional life-science training activities |

---

## Your access pathway {#pathways}

Choose the pathway that matches your use case.

### Research, development, and self-paced learning

Apply through [ABLeS](https://australianbiocommons.github.io/ables/eligibility). Your
application should describe your purpose, expected duration, scale of compute needed, and any
data considerations.

[AUTHOR TO SUPPLY — link to BioShell-specific access request form once available]

### Training events

If you are planning a workshop or training event using BioShell, contact the
[BioCommons Training Team](https://www.biocommons.org.au/event-support) or email
[comms@biocommons.org.au](mailto:comms@biocommons.org.au).

BioShell training events must meet Australian BioCommons Co-hosted Event standards, including:

- Open to all suitable Australian life science researchers
- Affiliated with an Australian research organisation or university
- Publicly advertised at least one month before the event
- Delivered by an expert practitioner with clear learning outcomes
- Content reviewed by the Australian BioCommons Training and Communications team
- Adherence to the [Australian BioCommons Code of Conduct](https://zenodo.org/record/4276854)

> **Tip:** Visit the [Event support page](https://www.biocommons.org.au/event-support) to see
> the full range of events Australian BioCommons can support.

---

## Access duration and renewal {#duration}

Research and development project access is granted for 3 months and is renewable. Renewal
requires demonstrated usage and continued progress against the aims stated in your original
application.

> **Note:** Training access duration is determined on a case-by-case basis and is coordinated
> by the BioCommons Training Team. Contact
> [comms@biocommons.org.au](mailto:comms@biocommons.org.au) for details.

---

## Connecting to BioShell {#connecting}

Once your environment is provisioned you will receive connection details by email.

### Step 1 — Generate an SSH key {#ssh-key}

BioShell uses SSH key authentication. If you do not already have an SSH key pair, generate
one on your local machine before connecting.

**Quick start** (works on macOS, Linux, and Windows with OpenSSH):

```bash
ssh-keygen -t ed25519 -C "user@example.com" -f ~/.ssh/bioshell_key
```

Replace `user@example.com` with your own email address. Accept the default prompts, or set
a passphrase when asked (recommended).

**Complete setup — macOS:**

```bash
# Create .ssh directory if it doesn't exist
mkdir -p ~/.ssh && chmod 700 ~/.ssh

# Generate the key
ssh-keygen -t ed25519 -C "user@example.com" -f ~/.ssh/bioshell_key

# Start ssh-agent and load the key into the macOS Keychain
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/bioshell_key

# Set correct permissions on the private key
chmod 600 ~/.ssh/bioshell_key

# Copy the public key to your clipboard
pbcopy < ~/.ssh/bioshell_key.pub
```

**Complete setup — Linux:**

```bash
# Create .ssh directory if it doesn't exist
mkdir -p ~/.ssh && chmod 700 ~/.ssh

# Generate the key
ssh-keygen -t ed25519 -C "user@example.com" -f ~/.ssh/bioshell_key

# Start ssh-agent and add the key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/bioshell_key

# Set correct permissions on the private key
chmod 600 ~/.ssh/bioshell_key

# Copy the public key to your clipboard (requires xclip)
xclip -selection clipboard < ~/.ssh/bioshell_key.pub
```

**Complete setup — Windows (PowerShell with OpenSSH):**

```powershell
# Generate the key
ssh-keygen -t ed25519 -C "user@example.com" -f "$env:USERPROFILE\.ssh\bioshell_key"

# Start ssh-agent service and add the key
Start-Service ssh-agent
ssh-add "$env:USERPROFILE\.ssh\bioshell_key"

# Copy the public key to your clipboard
Get-Content "$env:USERPROFILE\.ssh\bioshell_key.pub" | Set-Clipboard
```

> **Note:** OpenSSH ships with Windows 10 (version 1809 and later) and Windows 11. If
> `ssh-keygen` is not found, go to **Settings → Apps → Optional features** and install
> **OpenSSH Client**.

> **Tip:** Your public key is the `.pub` file — this is what you share with others or
> submit when registering for access. Never share your private key (the file without `.pub`).

### Step 2 — Submit your public key {#submit-key}

[AUTHOR TO SUPPLY — confirm how users submit their public key as part of the BioShell
provisioning process, e.g. via the access request form or a separate step after approval]

### Step 3 — Connect {#connect}

Add an entry to `~/.ssh/config` so SSH automatically uses your BioShell key without
needing to specify it each time:

```
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

Once connected, you can also open interactive environments directly in your browser:

- **JupyterLab** — `http://<your-bioshell-ip>:8888`
- **RStudio** — `http://<your-bioshell-ip>:8787`

> **Important:** Your connection details are provided as part of your provisioning
> notification. Keep them secure and do not share them publicly.

> **Tip:** If you are connecting from outside your institution's network you may need a VPN
> or SSH tunnel. Contact your local IT support if you are unsure.
