---
title: Create an SSH key
description: How to generate an SSH key pair on macOS, Linux, and Windows to authenticate with BioShell.
---

BioShell uses SSH key authentication for access. You generate a key pair once on your own machine, then
share the public half with the BioShell team. It takes about two minutes.

## Step 1: Generate your key files {#generate}

### macOS and Linux

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

Press **Enter** three times to accept the defaults, then print your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

### Windows

```powershell
ssh-keygen -t ed25519 -C "your@email.com"
```

Press **Enter** three times to accept the defaults, then print your public key:

```powershell
cat $env:USERPROFILE\.ssh\id_ed25519.pub
```

{% include callout.html type="tip" content="Pressing **Enter** at the passphrase prompt creates a key with no passphrase, which is fine to start with. Adding a passphrase encrypts your private key at rest and is worth doing if you can." %}


## Step 2: Check your key files {#verify}

Two files are created. Only ever share the `.pub` one.

| File | Description |
|------|-------------|
| `id_ed25519` | Private key — never share this |
| `id_ed25519.pub` | Public key — this is what you send to BioShell |

Confirm both exist:

```bash
ls -la ~/.ssh/            # macOS and Linux
```

```powershell
dir $env:USERPROFILE\.ssh\   # Windows
```


## Step 3: Share your public key with BioShell {#submit}

Once your project application has been approved, email your **public key** to
BioShell admin email so your account can be set up.

1. Print your public key using the `cat` command from the [quick start](#quick-start) above.
2. Copy the whole line — it starts with `ssh-ed25519` and ends with your email address.
3. Paste it into the body of your email. Do not attach the private key file.

Your BioShell username is your first initial followed by your surname, all lowercase — so
Berenice Ioshell becomes `bioshell`. You will receive your
connection details by email once your environment is provisioned, then head to
[Connecting to BioShell](connecting) to log in for the first time.

{% include callout.html type="important" content="If you run into any issues, contact [AUTHOR TO SUPPLY — helpdesk email or link]." %}
