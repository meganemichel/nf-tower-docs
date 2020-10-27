---
title: Implicit variables
aliases:
- "/docs/nextflow-scripting"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
private: false
menu:
  docs:
    parent: Nextflow scripting
    weight: 3
---

## Script implicit variables

The following variables are implicitly defined in the script global execution scope:

| Name         | Description                                                                                                                                                |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `baseDir`    | The directory where the main workflow script is located (deprecated in favour of `projectDir` since `20.04.0`).                                            |
| `launchDir`  | The directory where the workflow is run (requires version `20.04.0` or later).                                                                             |
| `moduleDir`  | The directory where a module script is located for DSL2 modules or the same as `projectDir` for a non-module script (requires version `20.04.0` or later). |
| `nextflow`   | Dictionary like object representing nextflow runtime information (see `metadata-nextflow`).                                                                |
| `params`     | Dictionary like object holding workflow parameters specifing in the config file or as command line options.                                                |
| `projectDir` | The directory where the main script is located (requires version `20.04.0` or later).                                                                      |
| `workDir`    | The directory where tasks temporary files are created.                                                                                                     |
| `workflow`   | Dictionary like object representing workflow runtime information (see `metadata-workflow`).                                                                |

## Configuration implicit variables

The following variables are implicitly defined in the Nextflow configuration file:

| Name         | Description                                                                                                     |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| `baseDir`    | The directory where the main workflow script is located (deprecated in favour of `projectDir` since `20.04.0`). |
| `launchDir`  | The directory where the workflow is run (requires version `20.04.0` or later).                                  |
| `projectDir` | The directory where the main script is located (requires version `20.04.0` or later).                           |

## Process implicit variables

In the process definition scope it's available the `task` implicit variable which allow accessing the current task configuration directives. For examples:

{{< highlight groovy >}}
    process foo {
      script:
      """
      some_tool --cpus $task.cpus --mem $task.memory
      """
    }
{{< /highlight >}}

In the above snippet the `task.cpus` report the value for the `cpus directive<process-cpus>` and the `task.memory` the current value for `memory directive<process-memory>` depending the actual setting given in the workflow configuration file.

See `Process directives <process-directives>` for details.
