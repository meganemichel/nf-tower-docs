---
title: Creating a Compute Environment
aliases:
- "/docs/compute-environments"
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
    parent: Compute Environments
    weight: 1
---

Tower uses a concept of **Compute Environments** to define the execution platform where a pipeline will run.

Tower supports launching of pipelines into **AWS Batch**, **Google Cloud**, **IBM LSF**, and **Slurm** with many more in development.

{{% pretty_screenshot img="/uploads/2020/09/compute_env_platforms.png" %}}

Each compute environment must be configured to enable Tower to submit tasks. You can read more on how to set up each environment using the links below.

## Select a default compute environment

If you have more than one **Compute Environment** you can select which one will be selected by default when launching a pipeline.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env.png" %}}

**1.** Navigate to your [compute environments page](https://tower.nf/compute-envs).

{{% pretty_screenshot img="/uploads/2020/10/compute_env_make_primary.png" %}}

**2.** Select your default environment by clicking on the **Make primary** button.   


### Quick Setup Guides

* [AWS Batch](/docs/compute-environments/aws-batch/)
* [Google Cloud](/docs/compute-environments/google-cloud/)
* [IBM LSF](/docs/compute-environments/lsf/)
* [Slurm](/docs/compute-environments/slurm/)
