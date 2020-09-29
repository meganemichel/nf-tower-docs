---
title: SLURM
weight: 3
publishdate: 2017-12-31T04:00:00.000+00:00
expirydate: 2030-01-01T04:00:00.000+00:00
date: '2017-04-06T04:00:00.000+00:00'
layout: single
images:
- "/uploads/2018/01/OGimage-01-docs-3x.jpg"
menu:
  docs:
    parent: Compute Environments
    weight: 4

---
[*Slurm is an open source, fault-tolerant, and highly scalable cluster management and job scheduling system for large and small Linux clusters.*](https://slurm.schedmd.com/overview.html)

{{% warning %}}
Note: support for remote batch schedulers is an incubating feature
This feature allows Tower to connect to your local cluster and launch the execution of the requested pipeline. The following requirements need to be fulfilled:

1. The cluster should be reachable via an SSH connection using an SSH key
2. The cluster should allow outbound connections to the Tower web service
3. The cluster queue used to run the Nextflow head job must be able submit cluster jobs
4. Nextflow runtime version 20.08.1-edge (or later) should be pre-installed in the cluster
{{% /warning %}}



## Slurm workload manager

To create a new compute environment for AWS, follow these steps:

{{% pretty_screenshot img="/uploads/2020/09/new_env.png" %}}

In the navigation bar on the upper right, choose your account name then choose "Compute environments". Click on the *New Environment* button.

{{% pretty_screenshot img="/uploads/2020/09/slurm_new_env.png" %}}

Choose a descriptive name for this environment. For example "Slurm env" and Select **Slurm Workload MAnager** as the target platform

{{% pretty_screenshot img="/uploads/2020/09/slurm_tower_credentials.png" %}}


Click the **+** sign to add new Credentials. Name your credentials and enter your **SSH private key** and associated **Passphrase**. Then Click **Create**

{{% tip %}}
Note the SSH passphrase can be optional.
{{% /tip %}}

{{% pretty_screenshot img="/uploads/2020/09/slurm_tower_options.png" %}}


Enter the absolute path of the **Work** and **Launch directories**, the **Username** on the cluster used to launch the pipeline execution, the **Login hostname** (which is usually is the cluster login node), the **Head queue name** which is the name of the queue on the cluster used to launch the execution of the Nextflow pipeline, and the **Compute queue name** which is the name of queue on the cluster to which pipeline jobs are submitted. Then click **Create**.

{{% tip %}}
This queue can be overridden by the pipeline configuration.
{{% /tip %}}
