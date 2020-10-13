---
title: Re-launch
aliases:
- "/docs/launch/re-launch"
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
    weight: 3
---

Re-launching pipelines is great way to quickly troubleshoot or make use of Nextflow's resume functionality and re-launch the same pipeline with different parameters.

{{% pretty_screenshot img="/uploads/2020/10/launch_relaunch.png" %}}

<br>

The Resume option is selected by default when re-launching a new pipeline. In short, The `-resume` option allows for the continuation of a workflow execution.

{{% pretty_screenshot img="/uploads/2020/10/launch_resume.png" %}}

<br>

{{% tip %}}
for a detailed explanation of how the resume option works please visit [part 1](https://www.nextflow.io/blog/2019/demystifying-nextflow-resume.html) and [part 2](https://www.nextflow.io/blog/2019/troubleshooting-nextflow-resume.html) in the `Demystifying Nextflow resume` story in the [Nextflow blog](https://www.nextflow.io/blog)
{{% /tip %}}
