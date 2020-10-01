---
title: Workflow Execution
aliases:
- "/docs/launch"
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
    parent: Launching Pipelines
    weight: 1
---

Pipelines are executed in the users own computing environment. Nextflow Tower currently has support for AWS, Google, Slurm or LSF. Visit [this page](/content/docs/compute-environments) to configure an environment.

{{% pretty_screenshot img="/uploads/2020/10/launch_button.png" %}}

**1.** Clcik on the **Launch** button in the navigation bar

The **Launch Pipeline** pop up window will appear with your default compute environment selected.

In the following example we will simply run the existing [RNASeq nf-core pipeline](https://nf-co.re/rnaseq)
with the ```-profile test,docker``` options.

The ```docker``` profile is a generic configuration which pulls a docker container from [dockerhub:nfcore/rnaseq](http://hub.docker.com/r/nfcore/rnaseq/).

The ```test``` profile is a complete configuration that includes links to test data. No other parameters are needed.

{{% pretty_screenshot img="/uploads/2020/10/launch_RNASeq.png" %}}

**1.** Note your primary compute environment is selected by default. To change it click on the drop down menu.
**2.** Enter the URL where the pipeline is hosted. In this case [`https://github.com/nf-core/rnaseq.git`](https://github.com/nf-core/rnaseq.git)
**3.** The **Revision number** gives you the option to select a different pipeline version. By default the stable (**master**) git branch will be used.
**4.** The **Work directory** must be the same bucket which was associated when creating your compute environment.

{{% warning %}}
To use an alternative bucket, create a new compute environment associated with it.
{{% /warning %}}

**5.** For each **Config profile** enter the name followed by the `Enter` key. Note we've added two profiles in this example.

**6.** Config parameters should be should be written in the following format:

        params.input='s3://tower-bucket/exome-data/ERR013140_{1,2}.fastq.bz2'  
        params.email='me@mail.msg'

{{% tip %}}
Note the full path to the s3 bucket.
{{% /tip %}}
