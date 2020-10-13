---
title: Advanced options
aliases:
- "/docs/launch/advanced"
weight: 5
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
    parent: Launching Pipelines
    weight: 2
---
Advanced launch options allow users to modify the configuration and execution of the pipeline.


## Nextflow config file
The *Nextflow config* field allows the addition of setting to the Nextflow configuration file. 

This text should follow the same syntax as the [Nextflow configuration file](https://www.nextflow.io/docs/latest/config.html?highlight=profiles#config-syntax). 

In teh example below, we can modify the **manifest** section to give the pipeline a name and description.

{{% pretty_screenshot img="/uploads/2020/10/launch_manifest.png" %}}

Notice that when monitoring the pipeline, the default name has been overwritten.

{{% pretty_screenshot img="/uploads/2020/10/launch_pipeline_rename.png" %}}

<br>

## Pre & post-run scripts

It is possible to run custom code, either before and after the execution of the Nextflow script. 

These fields allow users to enter BASH code.