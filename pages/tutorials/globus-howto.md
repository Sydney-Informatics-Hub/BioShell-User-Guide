---
title: How to move data in and out of BioShell
description: How to use globus to move data in/out of BioShell
---

You can use multiple data movement methods on BioShell, the right one for you will depend on your dataset and the locations you’re moving it to. There are multiple protocols and tools available for transferring data. Each has its strengths and limitations. 

| Method | Use case | Benefits | Limitations |
|---|---|---|---|
| scp/sftp | Small, ad-hoc file transfers | Simple, widely available | Fragile for large data, no automatic resume |
| rsync | Synchronising directories | Efficient for incremental updates | Manual retries, complex flags, not fault-tolerant |
| Browser (HTTP) downloads | Small public datasets | Easy for one-off use | Not suitable for large or private datasets |
| Managed transfer services (Globus and FileSender) | Research-scale workflows | Reliable, scalable, auditable | Requires some setup and understanding |

## Preferred method: Globus Connect Personal 

To transfer data between BioShell and your personal laptop or institutional storage with Globus, we recommend [**Globus Connect Personal**](https://docs.globus.org/globus-connect-personal/) which you can also [**install on your personal computer**](https://docs.globus.org/globus-connect-personal/). You can confirm it is installed on your BioShell VM by running: 

```
module avail globusconnectpersonal
```

```
----------------------------- /apps/Modules/modulefiles -----------------------------
   globusconnectpersonal/3.2.9

```

This will present your laptop’s storage as a private collection in the Globus web app, which you can then transfer data to/from other Globus collections.

Load it with: 

```
module load globusconnectpersonal
```
```
Globus Connect Personal
----------------------------------
The first time you use Globus Connect Personal you must complete
setup before you can run the full application.
Interactive setup (requires a browser on this machine):
  globusconnectpersonal
Headless setup (generate a setup key at https://app.globus.org/file-manager/gcp):
  globusconnectpersonal -setup <setup key>
Once setup is complete:
  globusconnectpersonal -start &
  globusconnectpersonal -status
  globusconnectpersonal -stop
To keep your endpoint running after logout:
  systemctl --user enable --now globusconnectpersonal
```

### Set up your endpoint 

Before you can use Globus, you will need to complete the set up in your browser. Run:

```
globusconnectpersonal
```

You'll be presented with a link you can open in your browser. Follow instructions in the browser and select "Allow". 

Copy the authorisation code provided and paste it into your BioShell terminal. 

Give your BioShell VM endpoint a name: 

```
Input a value for the Endpoint Name: bioshell
```

Once your endpoint is set up, you can then run it with: 

```
globusconnectpersonal -start & 
```

{% include callout.html type="important" content="Using the **&** in `globusconnectpersonal -start & ` allows it to run in the background to ensure you can keep using your BioShell VM." %}

### Initiate a transfer 

You can use the Globus the web app to manage your transfers, see the [Globus documentation](https://docs.globus.org/guides/tutorials/manage-files/transfer-files/) for instructions.

### Stop Globus Connect Personal 

Successfully completed transfers will be notified by email. Once your transfer has completed, stop with:

```
./globusconnectpersonal -stop
```

The following message should appear:

```
Globus Connect Personal is currently running and connected to Globus Online
Sending stop signal... Done
```