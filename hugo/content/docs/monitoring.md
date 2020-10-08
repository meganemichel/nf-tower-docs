---
title: Tracking progress
aliases:
- "/docs/monitoring"
weight: 1
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
authors:
- Forestry Team
headline: ''
description: ''
textline: ''
categories: []
tags: []
cta:
  headline: ''
  textline: ''
  calls_to_action: []
private: false
images:
- "/uploads/2018/01/OGimage-01-docs-3x.jpg"
menu:
  docs:
    parent: Pipeline Monitoring
    weight: 1
---

## Overview

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


## Execution summary section

The top section contains details about the the job execution:

**1.** The Nextflow **Command line** to execute the job.

**2.** The **Parameters** include all parameters given in the arguments and from the selected profiles.

**3.** The **Configuration** contains all the information included in the configuration file. This includes the parameters.

**4.** Finally the **Execution log** tab is updated in real time with the logs from the master node running the Nextflow process.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_exec_log.png" %}}

## General section

The General panel has basic summary information on the environment and the job being executed:

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


## Status section

The **Status** section shows in real time the statuses of your workflow processes.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_status.png" %}}

## Processes section
In Nextflow a process is the basic processing primitive to execute a user script. The **Processes** section shows all processes and their instances (individual tasks). In this example, the [test profile](https://github.com/nf-core/rnaseq/blob/master/conf/test.config) is passing 4 fastq files as the `--reads` parameter.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_reads.png" %}}

We can see in the `main.nf` [file](https://github.com/nf-core/rnaseq/blob/3b6df9bd104927298fcdf69e97cca7ff1f80527c/main.nf#L829) the process `fastqc` is given the the four read files as input.

In Tower we can see there are 4 instances of the fastqc process.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_fastqc_processes.png" %}}

{{% tip %}}
Clicking in a processes filters it in the [Tasks section](## Tasks section) bellow.
{{% /tip %}}

## Aggregate stats

The **aggregate stats** section is a real-time summary of the resources used by the workflow.
{{% tip %}}
Note the estimated cost in only based on computation usage and does not currently take into account storage or associated network costs.
{{% /tip %}}


## Load and Utilization sections

As processes are being submitted to the compute environment, the **Load** and **Utilization** sections allow the user to monitor how many cores, memory and CPUs are being used as well as how many processes tasks are being computed.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_load.png" %}}

## Tasks section

The **Tasks** section shows all processes' tasks. You can use the `Search` bar to filter tasks by process name, tag, hash, status, etc. Clciking in previous sections' fields, e.g: "clicking in the `cached` card in the **Status** field will filter all the `cached` tasks"

{{% pretty_screenshot img="/uploads/2020/10/monitoring_cached.png" %}}

Clicking on a `Process` in the **Precesses** section will filter all tasks for that specific process

{{% pretty_screenshot img="/uploads/2020/10/monitoring_star.png" %}}

Clicking on a task gives specific information about that particular task including the `Command` used and the `Execution log`

{{% pretty_screenshot img="/uploads/2020/10/monitoring_task_command.png" %}}

This can be very helpful for troubleshooting. Note you can download the log files including `stdout` and `stderr` from your compute environment.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_task_exec_log.png" %}}

Furthermore, scrolling down shows details related to selected tasks including individual costs per task and resource usage.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_task_resources.png" %}}

## Metrics section

The last section shows plot with CPU, memory, Job duration and I/O usage grouped by Processes.  

{{% pretty_screenshot img="/uploads/2020/10/monitoring_metrics.png" %}}

{{% tip %}}
Hover your mouse over the box plots to display details
{{% /tip %}}
