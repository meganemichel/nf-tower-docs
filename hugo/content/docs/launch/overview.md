---
title: Launch overview
aliases:
- "/docs/launch/basics"
weight: 10
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
    weight: 1
---
{{% warning %}}
All pipelines are executed in a users own compute infrastructure. Tower currently supports launching into AWS, Google, Slurm and LSF compute environments. See the [**Compute Environment**](/content/docs/compute-environments) documentation to learn how to configure an environment.
{{% /warning %}}

To launch a pipeline:

**1.** Select the **Launch** button in the navigation bar.

{{% pretty_screenshot img="/uploads/2020/10/launch_button.png" %}}

<br>

The **Launch Pipeline** dialog will appear.  
*In the following we will launch the nf-core RNASeq pipeline.*

{{% pretty_screenshot img="/uploads/2020/10/launch_RNASeq.png" %}}

<br>

**2.** Select the drop down menu to choose a [**Compute Environment**](/content/docs/compute-environments).  
*A primary compute environment is selected by default.*

**3.** Enter the location of the **Pipeline to launch**.  
*For example https://github.com/nf-core/rnaseq.git*.

**3.** A **Revision number** can be used select different versions of pipeline.  
*The Git master branch or `manifest.defaultBranch` in the Nextflow configuration will be used by default.*

**4.** The **Work directory** specifies the location of the Nextflow work directory.  
*The location associated with the compute environment will be selected by default.*

**5.** Enter the name(s) of each of the Nextflow **Config profiles** followed by the `Enter` key.  
*See the Nextflow [Config profiles](https://www.nextflow.io/docs/latest/config.html?highlight=profiles#config-profiles) documentation for more details.*

**6.** Enter any **Config parameters** in YAML or JSON format.  
*For example:*

    reads: 's3://nf-bucket/exome-data/ERR013140_{1,2}.fastq.bz2'  
    paired_end: true

**7.** Select *Launch* to begin the pipeline execution.

{{% tip %}}
Nextflow pipelines are simply Git repositories and the location can be any public or private Git-hosting platform. See [**Git Integration**](/content/docs/git-integration) in the Tower docs and [**Pipeline Sharing**](https://www.nextflow.io/docs/latest/sharing.html) in the Nextflow docs for more details.
{{% /tip %}}

{{% warning %}}
The credentials associated with the compute environment must be able to access the work directory.
{{% /warning %}}

{{% tip %}}
In the configuration, the full path to the s3 bucket must be specified with single-quotes around strings no quotes around booleans or numbers.
{{% /tip %}}