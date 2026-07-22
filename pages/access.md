---
title: Accessing BioShell
type: Using BioShell
description:
  How to check your eligibility, request access to BioShell, and connect to your
  environment for the first time.
---

## Eligibility {#eligibility}

BioShell is a national resource funded through NCRIS. BioShell is available to researchers across **Australia**. Read the criteria below to determine whether your project aligns with our core principles and resource criteria. To qualify for access, your project must address the following:

**1. Domain and impact**
- Your work must have a molecular life sciences focus.
- Where appropriate, you are willing to openly share your results and outputs, including data, software, and methods. We celebrate this work on the [**BioShell community and outcomes**](community) page.

**2. Technical readiness and responsible resource use**
- Your team should have, or be actively building, the "hands-on" bioinformatics skills needed to make use of the environment.
- Your resource usage must be planned and proportional as national infrastructure is finite.

**3. Governance and leadership**
- You, or someone in your group, is able and willing to act as the project lead and be responsible for all resource usage and adherence to facility policies.

By submitting an access request you agree to the [**BioShell acceptable use policy and service terms**](aup), including the requirement to [acknowledge BioShell](aup#acknowledging-the-service) in relevant research, training, and innovation outputs.

<p style="text-align:center; margin-bottom: 3rem;"> <a href="https://docs.google.com/forms/d/e/1FAIpQLScOVZjZTvdZC65cnUN2BmasQhxYjTzDjJu2bleIZKQZyB7LrA/viewform?usp=dialog" class="btn btn-primary btn-lg" style="padding: 0.75rem 2.5rem;">Request access to BioShell</a></p>

{% include callout.html type="tip" content="BioShell runs on a single cloud environment, well suited to interactive work and moderate-scale pipelines. If you are running highly parallel or large batch jobs, a national HPC facility may be a better fit. The [**Australian BioCommons Leadership Share (ABLeS)**](https://australianbiocommons.github.io/ables/index) is a complementary programme that provides access to HPC infrastructure and expertise. To get a feel for the kinds of work each supports, browse some example [**BioShell**](community) and [**ABLeS**](https://australianbiocommons.github.io/ables/participants) projects, then see [**choosing an environment size**](flavours#beyond-single-environment) for some worked examples for choosing your environment." %}

## What's supported?

| User type            | Description                                                                   |
| -------------------- | ----------------------------------------------------------------------------- |
| Research projects    | Academic, government, or industry life-science research with defined outcomes |
| Development projects | Bioinformatics tool or workflow development intended for community use        |

### What does access include? {#quotas}

| Research | Development |
|---|---|---|---|
| Data analysis and visualisation | Yes | Yes |
| Interactive interfaces (JupyterLab, RStudio) | Yes | Yes |
| SSH CLI access | Yes | Yes |
| Storage quota | 100 GB (default) | 100 GB (default) |
| Allocation period | 3 months | 3 months |
| Renewal | Yes, subject to review | Yes, subject to review |


{% include callout.html type="note" content="The default storage quota is 100 GB, and you can request up to 1 TB with justification. Quotas are enforced at the infrastructure level, so it is not possible to exceed your allocation. To request more storage, contact [**BioShell support**](https://docs.google.com/forms/d/e/1FAIpQLSc6Tr2FAponrwYuMXfqspuzcXnssbM5gQ9ChLUqzh5yUxWJuQ/viewform). Requests are subject to availability and review and cannot be guaranteed." %}


### Access duration and renewal {#duration}

For research and development projects access is granted for 3 months and is renewable. Renewal
requires demonstrated usage and continued progress against the aims stated in your original
application. 

Check out the [**project renewal form**](https://docs.google.com/forms/d/e/1FAIpQLSdhQzizqnQ7f5Os7TCDpXviiIVs1nbmtWweDzGH4r0poZlxxw/viewform?usp=header) for more information or to make a request.

## Training events

If you are planning a workshop or training event and wish to use BioShell, contact the
[**BioCommons Training Team**](mailto:training@biocommons.org.au). Check out the Australian BioCommons [event support page](https://www.biocommons.org.au/event-support) for additional information on how we support training.

{% include callout.html type="note" content="BioShell is the goto environment for running command-line training for details about workshops that have been run on BioShell see the **BioShell community and outcomes page**." %}


## Connecting to BioShell {#connecting}

Once your environment is provisioned you will receive connection details by email. For
step-by-step SSH connection instructions and information about your first login, see the
[**Connecting to BioShell**](connecting) page.
