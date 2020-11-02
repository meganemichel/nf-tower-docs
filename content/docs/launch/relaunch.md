---
title: Re-launch
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Resuming Nextflow pipelines with Tower.nf'
description: 'Prescriptive guide to launch Nextflow pipelines using Tower.nf'
menu:
  docs:
    parent: Launching Pipelines
    weight: 3
---

Re-launching pipelines is great way to quickly troubleshoot or make use of Nextflow's resume functionality and re-launch the same pipeline with different parameters.

{{% pretty_screenshot img="/uploads/2020/10/launch_relaunch.png" %}}

The Resume option is selected by default when re-launching a new pipeline. In short, The `-resume` option allows for the continuation of a workflow execution.

{{% pretty_screenshot img="/uploads/2020/10/launch_resume.png" %}}

{{% note "Demystifying Nextflow resume" %}}
for a detailed explanation of how the resume option works please visit [part 1](https://www.nextflow.io/blog/2019/demystifying-nextflow-resume.html) and [part 2](https://www.nextflow.io/blog/2019/troubleshooting-nextflow-resume.html) in the `Demystifying Nextflow resume` story in the [Nextflow blog](https://www.nextflow.io/blog)
{{% /note %}}
