---
title: Dashboard Overview
aliases:
- "/docs/monitoring-tower-pipelines"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Monitoring Nextflow pipelines with Tower.nf'
description: 'Prescriptive guide to monitor Nextflow pipelines executed through Tower.nf'
menu:
  docs:
    parent: Monitoring Pipelines
    weight: 1
---

## Searching pipelines

{{% tip %}}
Use asterisks `✽` before and after keywords to search for job names and pipelines.  
{{% /tip %}}

{{% pretty_screenshot img="/uploads/2020/10/monitoring_search.png" %}}

## Left side navigation bar
{{% tip %}}
Note all information in the dashboard is updated in realtime.
{{% /tip %}}


Once Jobs have been submitted Tower enables you to easily monitor progress and results. **The left side bar** contains all previous jobs executions, each new or resumed job will be given a random name starting with an adjective and followed by a noun e.g(grave_williams).

{{% pretty_screenshot img="/uploads/2020/10/monitoring_overview.png" %}}

In the left bar:

  - jobs in **Blue** are running Jobs
  - jobs in **Green** are successfully executed Jobs
  - jobs in **yellow** are successfully executed Jobs where some process partially failed
  - jobs in **red** are jobs where where at least one process fully failed
  - jobs in **gray** are jobs that where forced to stop during execution

  Selecting a job on the left panel will display the job execution details.
