---
title: Individual Processes and Metrics
menu:
  docs:
    parent: Monitoring Pipelines
    weight: 6
---


The **Tasks** section shows all processes' tasks. You can use the `Search` bar to filter tasks by process name, tag, hash, status, etc. Clciking in previous sections' fields, e.g: "clicking in the `cached` card in the **Status** field will filter all the `cached` tasks"

{{% pretty_screenshot img="/uploads/2020/10/monitoring_cached.png" %}}

Clicking on a `Process` in the **Precesses** section will filter all tasks for that specific process

{{% pretty_screenshot img="/uploads/2020/10/monitoring_star.png" %}}

Clicking on a task gives specific information about that particular task including the `Command` used and the `Execution log`

{{% pretty_screenshot img="/uploads/2020/10/monitoring_task_command.png" %}}

This can be very helpful for troubleshooting. Note you can download the log files including `stdout` and `stderr` from your compute environment.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_task_exec_log.png" %}}

Furthermore, scrolling down shows details related to selected tasks including individual costs per task and resource usage.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_task_resources.png" %}}

## Processes' metrics 

The last section shows plot with CPU, memory, Job duration and I/O usage grouped by Processes.  

{{% pretty_screenshot img="/uploads/2020/10/monitoring_metrics.png" %}}

{{% tip %}}
Hover your mouse over the box plots to display details
{{% /tip %}}
