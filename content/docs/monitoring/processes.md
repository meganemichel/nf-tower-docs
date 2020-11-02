---
title: Processes
aliases:
- "/docs/monitoring/job-processes"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'List of the pipeline processes and their status'
description: 'Monitoring a Nextflow pipeline executed through Tower.nf'

menu:
  docs:
    parent: Monitoring Pipelines
    weight: 4
---

In Nextflow a `process` is the basic processing `primitive` to execute a user script. The **Processes** section shows all processes and their instances (individual tasks). In this example, the [test profile](https://github.com/nf-core/rnaseq/blob/master/conf/test.config) is passing 4 fastq files as the `--reads` parameter.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_reads.png" %}}

We can see in the `main.nf` [file](https://github.com/nf-core/rnaseq/blob/3b6df9bd104927298fcdf69e97cca7ff1f80527c/main.nf#L829) the process `fastqc` is given the the four read files as input.

In Tower we can see there are 4 instances of the fastqc process.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_fastqc_processes.png" %}}

{{% tip "Filtering processes" %}}
Clicking in a processes filters it in the [Tasks section](/docs/monitoring/tasks/) bellow.
{{% /tip %}}
