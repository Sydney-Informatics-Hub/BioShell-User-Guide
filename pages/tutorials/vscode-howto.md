---
title: Use VS Code with BioShell
type: Tutorial
description: How to install Visual Studio Code, add the Remote - SSH extension, and connect to your BioShell environment to edit files and run terminals.
---

[Visual Studio Code](https://code.visualstudio.com/) (VS Code) is a free code editor that can
connect to your BioShell environment over SSH. Once connected, you can browse directories, edit,
view and download files, and run a terminal inside BioShell from a single window on your own
computer.

Before you start, you will need:

- A BioShell environment and your connection details (your username and your BioShell IP address).
- An SSH key pair whose public key has been shared with the BioShell team. If you have not done
  this yet, follow the [**SSH key generation tutorial**](ssh-keys-howto) first.

{% include callout.html type="tip" content="If you can already connect from a terminal with `ssh bioshell` (see [Connecting to BioShell](connecting)), VS Code will reuse that same SSH configuration." %}


## Step 1: Install VS Code {#install}

1. Download VS Code by following the [installation instructions](https://code.visualstudio.com/docs/setup/setup-overview) for your operating system.
2. Open VS Code to confirm it installed correctly.

   ![](assets/img/vscode_welcome.png)

   *The VS Code welcome screen after a fresh install.*


## Step 2: Install the extensions {#extensions}

The **Remote - SSH** extension lets VS Code connect to BioShell.

1. In the left sidebar, select the **Extensions** button (four blocks), or press `Ctrl+Shift+X` (`Cmd+Shift+X` on macOS).
2. Search for `remote ssh` and select **Remote - SSH** (published by Microsoft).
3. Select **Install**.

   ![](assets/img/vscode_remote_ssh_extension.png)

   *Top: (1) the **Extensions** button in the left sidebar; (2) the **Install** button for Remote - SSH. Bottom, after installing: the button changes to **Disable** and **Uninstall**, and (3) the new **Remote Explorer** icon appears in the left sidebar.*

You can install other extensions in the same way to suit your work. For example, if you
[use Nextflow in BioShell](nextflow-howto), search for `nextflow` and install the **Nextflow**
extension for syntax highlighting.

{% include callout.html type="note" content="Extensions that work with files (such as language support) are installed into your BioShell environment the first time you use them while connected. VS Code will prompt you to **Install in SSH: bioshell** if needed." %}


## Step 3: Add BioShell as an SSH host {#ssh-config}

VS Code reads hosts from your SSH config file (`~/.ssh/config`). If you already added a
`bioshell` entry when [connecting to BioShell](connecting#ssh-connect), skip to
[Step 4](#connect).

1. Press `Ctrl+Shift+P` (`Cmd+Shift+P` on macOS) to open the **Command Palette**.
2. Type `remote ssh` and select **Remote-SSH: Open SSH Configuration File...**. This may appear in a different position in the list.

   ![](assets/img/vscode_open_config.png)

   *The Command Palette filtered by `remote ssh`. The arrow points to **Remote-SSH: Open SSH Configuration File...**.*

3. Select your user config file (for example `~/.ssh/config` on macOS and Linux, or `C:\Users\<you>\.ssh\config` on Windows).

   ![](assets/img/vscode_select_config.png)

   *The arrow points to the config file in your own home directory (`/Users/<you>/.ssh/config` on macOS). Do not select the system-wide `/etc/ssh/ssh_config`.*

4. Add the following entry, replacing the placeholders with your connection details and the path to your private key:

   ```text
   Host bioshell
       HostName <your-bioshell-ip>
       User <username>
       IdentityFile ~/.ssh/bioshell_key
   ```

5. Save the file.

{% include callout.html type="tip" content="Alternatively, choose **Remote-SSH: Add New SSH Host...** and enter `ssh -i ~/.ssh/bioshell_key <username>@<your-bioshell-ip>`. VS Code writes the entry for you, but names it after the IP address rather than `bioshell`." %}


## Step 4: Connect to BioShell {#connect}

1. Open the **Command Palette** (`Ctrl+Shift+P`, or `Cmd+Shift+P` on macOS).
2. Type `remote ssh` and select **Remote-SSH: Connect to Host...**. This may appear in a different position in the list.

   ![](assets/img/vscode_connect_host.png)

   *The Command Palette filtered by `remote ssh`. The arrow points to **Remote-SSH: Connect to Host...**.*

3. Select **bioshell**. A new VS Code window opens.

   ![](assets/img/vscode_connect_bioshell.png)

   *The list of configured SSH hosts. The arrow points to `bioshell`, the host you added to your SSH config file.*

{% include callout.html type="important" content="The first time you connect you may be asked to confirm a few settings:<br>• If you see `<host> has fingerprint ...`, select **Continue**.<br>• If you are asked for the remote platform, select **Linux**.<br>• If your private key has a passphrase, enter it when prompted." %}

When the indicator in the bottom-left corner of the window shows **SSH: bioshell**, you are
connected. The first connection can take a minute while VS Code sets up its server component in
your home directory.

![](assets/img/vscode_connected.png)

*The **SSH: bioshell** indicator in the bottom-left corner confirms you are connected to BioShell.*


## Step 5: Open a folder and a terminal {#workspace}

1. Select the **Explorer** in the left sidebar (two pages icon), or press `Ctrl+Shift+E` (`Cmd+Shift+E` on macOS).
2. Select **Open Folder**.
3. Enter the folder you want to work in and select **OK**. Your home directory (`/home/<username>/`) is the default; to work with your analysis data, enter `/mnt/data` instead.

   ![](assets/img/vscode_open_folder.png)

   *(1) the **Remote Explorer** icon in the left sidebar; (2) the **Open Folder** button in the Explorer; (3) the folder path box, here set to `/mnt/data`. Select **OK** to open it.*

4. If prompted, select **Yes, I trust the authors**.
5. Open a terminal inside BioShell by pressing `` Ctrl+` `` (`` Cmd+` `` on macOS), or from the menu with **Terminal > New Terminal**.

The folder appears in the Explorer sidebar, where you can open, edit, upload (drag and drop) and
download (right-click > **Download...**) files. Commands typed in the terminal run in your
BioShell environment, so tools and modules work exactly as they do over a plain SSH connection.

![](assets/img/vscode_folder_terminal.png)

*The **Folder** label points to the opened `/mnt/data` folder, shown as `data [SSH: bioshell]` in the Explorer sidebar. The **Terminal** label points to a terminal running inside BioShell, with the prompt in `/mnt/data`.*

{% include callout.html type="tip" content="Store your analysis inputs and outputs on the data volume at `/mnt/data` rather than in your home directory. See [Storage volumes](connecting#storage) for details." %}

{% include callout.html type="important" content="If you run into any issues, fill in a [support request form](https://docs.google.com/forms/d/e/1FAIpQLSc6Tr2FAponrwYuMXfqspuzcXnssbM5gQ9ChLUqzh5yUxWJuQ/viewform?usp=dialog)." %}
