---
title: Interactive environments
description: How to use JupyterLab and RStudio browser-based coding environments in BioShell.
---

BioShell supports two browser-based interactive environments for notebook and script-based
work. Both run on your BioShell instance and are accessible through your browser once you
have connected via SSH.

> **Important:** You must have an active SSH connection to your BioShell instance before
> opening either environment in your browser. See [Connecting to BioShell](/access#connecting).

---

## JupyterLab {#jupyterlab}

JupyterLab is a browser-based environment for writing and running Python, R, and shell
notebooks. It is well suited to exploratory analysis, visualisation, and combining code
with documentation.

Open your browser and go to:

```
http://<your-bioshell-ip>:8888
```

![](images/bioshell/SCREENSHOT_NEEDED_jupyterlab.png)

*Fig 4. JupyterLab open in a browser connected to a BioShell instance.*

---

## RStudio {#rstudio}

RStudio is a browser-based environment for R-based analysis. If you are familiar with the
RStudio desktop application, the browser version works in exactly the same way.

Open your browser and go to:

```
http://<your-bioshell-ip>:8787
```

![](images/bioshell/SCREENSHOT_NEEDED_rstudio.png)

*Fig 5. RStudio open in a browser connected to a BioShell instance.*

---

> **Tip:** Your SSH terminal and browser environments connect to the same BioShell instance.
> Any tool you have loaded with `module load` in your terminal is also available inside
> JupyterLab and RStudio.

> **Note:** If you cannot reach JupyterLab or RStudio in your browser, check that your SSH
> connection is still active. If you are connecting from outside your institution's network,
> you may need an SSH tunnel. Contact your local IT support for help with this.
