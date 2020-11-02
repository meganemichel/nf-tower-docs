---
title: Advanced options
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Advanced options for launching Nextflow pipelines with Tower.nf'
description: 'Prescriptive guide to launch Nextflow pipelines using Tower.nf'
menu:
  docs:
    parent: Launching Pipelines
    weight: 2
---
Advanced launch options allow users to modify the configuration and execution of the pipeline.


## Nextflow config file
The *Nextflow config* field allows the addition of settings to the Nextflow configuration file.

This text should follow the same syntax as the [Nextflow configuration file](https://www.nextflow.io/docs/latest/config.html?highlight=profiles#config-syntax).

In the example below, we can modify the **manifest** section to give the pipeline a name and description.

{{% pretty_screenshot img="/uploads/2020/10/launch_manifest.png" %}}

{{% tip "Pre & post-run scripts"%}}

It is possible to run custom code, either before and after the execution of the Nextflow script. These fields allow users to enter BASH code.

{{% /tip %}}


Notice that when monitoring the pipeline, the default name has been overwritten.

{{% pretty_screenshot img="/uploads/2020/10/launch_pipeline_rename.png" %}}
