---
title: Summary & status
aliases:
- "/docs/monitoring/job-summary"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Job summary & real-time status'
description: 'Monitoring a Nextflow pipeline executed through Tower.nf'
menu:
  docs:
    parent: Monitoring Pipelines
    weight: 3
---
The General summary displays information on the environment and the job being executed:

  - A unique ID
  - A Job name
  - Date and time of job submission
  - The Nextflow session ID
  - The username
  - The work directory path
  - The docker container image
  - The executor
  - The compute environment details
  - The Nextflow version

{{% tip %}}
hoover over with mouse to get full details.
{{% /tip %}}

{{% pretty_screenshot img="/uploads/2020/10/monitoring_general.png" %}}

### Status section

The **Status** section shows in real time the statuses of your workflow processes. The panel uses the same colour code as the pipelines in the left-side navigation bar.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_status.png" %}}
