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

**6.** Config parameters should be should be written in YAML or JSON format:

        input: 's3://tower-bucket/exome-data/ERR013140_{1,2}.fastq.bz2'  
        email: 'me@mail.msg'
        paired_end: true

{{% tip %}}
Note the full path to the s3 bucket, quotes around strings, and no quotes around booleans or numbers.
{{% /tip %}}

**7.** The advanced options allow us to add content to the configuration file. In this example we edit the **manifest** section to overwrite the Pipeline name and it's description. We can also point to a different main nextflow script, and a different git version.

{{% pretty_screenshot img="/uploads/2020/10/launch_manifest.png" %}}

Note the name of the pipeline is overwritten.

{{% pretty_screenshot img="/uploads/2020/10/launch_pipeline_rename.png" %}}

## Re-launching Pipelines

The re-launching option is a great way to troubleshoot pipelines or to re-launch the same pipeline with different parameters.

{{% pretty_screenshot img="/uploads/2020/10/launch_relaunch.png" %}}

Note the Resume option is selected by default when re-launching a new pipeline. In short, The `-resume` option allows for the continuation of a workflow execution.

{{% pretty_screenshot img="/uploads/2020/10/launch_resume.png" %}}

{{% tip %}}
for a detailed explanation of how the resume option works please visit [part 1](https://www.nextflow.io/blog/2019/demystifying-nextflow-resume.html) and [part 2](https://www.nextflow.io/blog/2019/troubleshooting-nextflow-resume.html) in the `Demystifying Nextflow resume` story in the [Nextflow blog](https://www.nextflow.io/blog)
{{% /tip %}}

## Pipeline email notifications

You can receive email notifications on workflow completion. Go to your profile page on the top right and select the `Send notification email on workflow completion` option at the bottom of the page.

{{% pretty_screenshot img="/uploads/2020/10/launch_notifications.png" %}}
