---
title: Generating an SSH key
description: How to generate an SSH key pair on macOS, Linux, and Windows to authenticate with BioShell.
---

BioShell uses SSH key authentication. This guide walks you through generating a key pair,
copying your public key, and submitting it to the BioShell Onboarding Portal.


## Quick start {#quick-start}

If you are familiar with SSH keys and just need the commands, use the steps below. For a
full walkthrough including optional settings and troubleshooting, see the
[step-by-step guide](#prerequisites) below.


### macOS and Linux

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

Press **Enter** three times to accept the defaults, then copy your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Paste the output into the [BioShell Onboarding Portal](AUTHOR TO SUPPLY — add URL).


### Windows

```powershell
ssh-keygen -t ed25519 -C "your@email.com"
```

Press **Enter** three times to accept the defaults, then copy your public key:

```powershell
cat $env:USERPROFILE\.ssh\id_ed25519.pub
```

Paste the output into the [BioShell Onboarding Portal](AUTHOR TO SUPPLY — add URL).


## Prerequisites {#prerequisites}

| Platform | Requirement |
|----------|-------------|
| macOS | Built-in (macOS 10.13+), no install needed |
| Linux | `openssh-client` — install with `sudo apt install openssh-client` or `sudo dnf install openssh` |
| Windows | OpenSSH Client — built-in on Windows 10 (1809+) and Windows 11; enable via **Settings > Optional Features** if missing |


## Step 1: Generate the key pair {#generate}

The recommended algorithm is **Ed25519** — it is fast, secure, and compact. Use RSA 4096
only if a service requires it.


### macOS and Linux

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```

**RSA fallback (legacy systems only):**

```bash
ssh-keygen -t rsa -b 4096 -C "your@email.com"
```

### Windows

```powershell
ssh-keygen -t ed25519 -C "your@email.com"
```

**RSA fallback:**

```powershell
ssh-keygen -t rsa -b 4096 -C "your@email.com"
```

### Responding to prompts

After running the command you will see two prompts:

```
Enter file in which to save the key (/home/you/.ssh/id_ed25519):
```

Press **Enter** to accept the default path. If you use multiple keys across different
systems, enter a custom path to keep them separate — for example:

```
/home/you/.ssh/id_ed25519_bioshell      # macOS and Linux
C:\Users\you/.ssh/id_ed25519_bioshell   # Windows
```

Your files will then be named `id_ed25519_bioshell` and `id_ed25519_bioshell.pub`. If you
use a custom name, reference it explicitly when copying your public key (Step 4) and in your
SSH config (see [Managing multiple keys](#multiple-keys)).

```
Enter passphrase (empty for no passphrase):
```

A passphrase encrypts your private key at rest. It is recommended for any key that will
access production systems. Press **Enter** to skip.


## Step 2: Verify the key files {#verify}

After generation, two files are created:

| File | Description |
|------|-------------|
| `id_ed25519` | Private key — never share this |
| `id_ed25519.pub` | Public key — share this with servers and services |


### macOS and Linux

```bash
ls -la ~/.ssh/
```

### Windows

```powershell
dir $env:USERPROFILE\.ssh\
```


## Step 3: Add the key to ssh-agent (optional) {#ssh-agent}

If you set a passphrase, the ssh-agent saves you from re-entering it every time you connect.
Run the relevant command once after login:

| Platform | Command |
|----------|---------|
| macOS | `ssh-add --apple-use-keychain ~/.ssh/id_ed25519` |
| Linux | `ssh-add ~/.ssh/id_ed25519` |
| Windows (PowerShell) | `ssh-add $env:USERPROFILE\.ssh\id_ed25519` |

{% include callout.html type="note" content="On macOS the key is stored in Keychain and persists across reboots. On Linux you may need to re-add the key after each session unless you configure your shell to start the agent automatically." %}


## Step 4: Copy your public key {#copy-key}


### macOS

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

Or print it to copy manually:

```bash
cat ~/.ssh/id_ed25519.pub
```


### Linux

```bash
# With xclip installed
xclip -selection clipboard < ~/.ssh/id_ed25519.pub

# Or print it to copy manually
cat ~/.ssh/id_ed25519.pub
```


### Windows

```powershell
# Copy to clipboard
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub | Set-Clipboard

# Or print it to copy manually
cat $env:USERPROFILE\.ssh\id_ed25519.pub
```


## Step 5: Submit your public key to BioShell {#submit}

1. Go to the [BioShell Onboarding Portal](AUTHOR TO SUPPLY — add URL).
2. Log in with your institutional credentials.
3. Paste your public key into the **SSH Public Key** field.
4. Submit the form — your account will be provisioned within [AUTHOR TO SUPPLY — add timeframe].

Your BioShell username will follow the convention `firstname.lastname`. Once your account is
ready, return to the [Connecting to BioShell](access#connect) section of the access guide.

{% include callout.html type="important" content="If you run into any issues, contact [AUTHOR TO SUPPLY — helpdesk email or link]." %}


## Managing multiple keys {#multiple-keys}

Use `~/.ssh/config` to assign different keys to different hosts:

```
Host bioshell
    HostName BIOSHELL_HOSTNAME
    User firstname.lastname
    IdentityFile ~/.ssh/id_ed25519_bioshell

Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github
```

With this config, connect to BioShell with `ssh bioshell`.

{% include callout.html type="note" content="On Windows this file lives at `C:\Users\you\.ssh\config`." %}


## Key permissions (macOS and Linux) {#permissions}

SSH will refuse to use keys with overly permissive file modes. Fix incorrect permissions with:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/config           # if it exists
chmod 600 ~/.ssh/authorized_keys  # on servers
```


## Adding a key later {#add-key}

If you need to register a new key (for example, from a new machine or after rotating an old
key), use one of these options:

**Option A: via the portal**

Log back into the [BioShell Onboarding Portal](AUTHOR TO SUPPLY — add URL) and add your new
public key there.

**Option B: directly on BioShell** (if you still have access via an existing key)

```bash
cat ~/.ssh/id_ed25519.pub | ssh firstname.lastname@BIOSHELL_HOSTNAME \
  "cat >> ~/.ssh/authorized_keys"
```

Then verify correct permissions on the server:

```bash
ssh firstname.lastname@BIOSHELL_HOSTNAME "chmod 600 ~/.ssh/authorized_keys"
```


## Testing your connection {#testing}

```bash
ssh firstname.lastname@BIOSHELL_HOSTNAME
```

Use `-v` for verbose output when troubleshooting:

```bash
ssh -v firstname.lastname@BIOSHELL_HOSTNAME
```


## Algorithm reference {#algorithms}

| Algorithm | Flag | Recommended? | Notes |
|-----------|------|--------------|-------|
| Ed25519 | `-t ed25519` | Yes | Modern default; fast and secure |
| RSA 4096 | `-t rsa -b 4096` | Fallback only | Use only if Ed25519 is unsupported |
| ECDSA | `-t ecdsa` | Avoid | Weaker than Ed25519 in practice |
| DSA | `-t dsa` | No | Deprecated; disabled in modern OpenSSH |
