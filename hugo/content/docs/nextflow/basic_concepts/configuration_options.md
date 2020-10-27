---
title: Configuration options
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
private: false
headline: 'Configuration options'
menu:
  docs:
    parent: Basic concepts
    weight: 4
---

Pipeline configuration properties are defined in a file named `nextflow.config` in the pipeline execution directory.

This file can be used to define which executor to use, the process's environment variables, pipeline parameters etc.

A basic configuration file might look like this:

{{< highlight groovy >}}
    process {
      executor='sge'
      queue = 'cn-el6'
    }
{{< /highlight >}}

Read the [config-page](/docs/config-page) section to learn more about the Nextflow configuration file and settings.
