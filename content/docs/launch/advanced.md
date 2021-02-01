---
title: Advanced options
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"

headline: 'Advanced launch options'
description: 'Advanced guide to launch Nextflow pipelines'
menu:
  docs:
    parent: Launching Pipelines
    weight: 2
---
{{% pretty_screenshot img="/uploads/2021/02/launch_advanced-options_overview.png" %}}

Advanced launch options allow users to modify the configuration and execution of the pipeline.


## Nextflow config file
The *Nextflow config* field allows the addition of settings to the Nextflow configuration file.

This text should follow the same syntax as the [Nextflow configuration file](https://www.nextflow.io/docs/latest/config.html?highlight=profiles#config-syntax).

In the example below, we can modify the **manifest** section to give the pipeline a name and description which will show up in the Tower monitoring section.

{{% pretty_screenshot img="/uploads/2020/10/launch_manifest.png" %}}

<br>

After changing the name in the manifest, when monitoring the pipeline, the name has been overwritten.

{{% pretty_screenshot img="/uploads/2020/10/launch_pipeline_rename.png" %}}

<br>

## Pre-run & Post-run scripts
These are optional Bash scripts which are executed before the pipeline is launched, and after the pipeline completion.
<br>
{{% tip %}}
Note the Post-run script will always run, whether the pipeline completes successfully or with an error condition. 
{{% /tip %}}
<br>
{{% pretty_screenshot img="/uploads/2021/02/launch_pre_post-run_scripts.png" %}}

## Pull latest Git verions
<br>
{{% pretty_screenshot img="/uploads/2021/02/launch_pull.png" %}}

The **Pull latest** option ensures Nextflow pulls the latest version from the Git repository. This is equivalent to using the `-latest` flag.

## Stub run
<br>
{{% pretty_screenshot img="/uploads/2021/02/launch_stub.png" %}}

The [stub option](https://nextflow.io/docs/edge/process.html#stub) is used inside a Nextflow Processes to mock the Process output. This option is used to only test the workflow logic.

## Main script 
<br>
{{% pretty_screenshot img="/uploads/2021/02/launch_main.png" %}}

Nextflow will attempt to run the script named `main.nf` in project repository by default. This can be changed via either the `manifest.mainScript` option or by providing the script filename to run in this field.

## Workflow entry name
<br>
{{% pretty_screenshot img="/uploads/2021/02/launch_workflow-entry.png" %}}

Nextflow DSL2 provides the ability to launch specific named workflows. Enter the name of the workflow to be executed in this field.