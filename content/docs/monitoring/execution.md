---
title: Execution details & logs
aliases:
- "/docs/monitoring/execution-logs"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Main window - job monitoring  & logs'
description: 'Monitoring a Nextflow pipeline executed through Tower.nf'
menu:
  docs:
    parent: Monitoring Pipelines
    weight: 2
---

Selecting a pipeline on the on the left side bar shows the workflow details on the main window. The main window contains:

* a [general top section](#job-execution-information) with execution details: the command line use, the parameters, the job configuration, and the execution logs in real-time.
* a [general summary](/docs/monitoring/general_section/) and [status section](/docs/monitoring/general_section/)
* a list of [processes]()
* aggregated [stats]()
* a summary of used [resources]()
* a detailed list of [individual tasks]()
* a [metrics dashboard]()

### Job execution information

This top section is composed of 4 tabs containing details about the the job execution:

**1.** The Nextflow **Command line** to execute the job.

**2.** The **Parameters** include all parameters given in the arguments and from the selected profiles.

**3.** The **Configuration** contains all the information included in the configuration file. This includes the parameters.

**4.** Finally the **Execution log** tab is updated in real time with the logs from the master node running the Nextflow process.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_exec_log.png" %}}