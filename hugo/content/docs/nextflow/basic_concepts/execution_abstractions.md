---
title: Execution abstraction
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
private: false
headline: 'Execution abstraction'
menu:
  docs:
    parent: Basic concepts
    weight: 2
---

While a process defines _what_ command or script has to be executed, the _executor_ determines _how_ that script is actually run on the target system.

If not otherwise specified, processes are executed on the local computer. The local executor is very useful for pipeline development and testing purposes, but for real world computational pipelines an HPC or cloud platform is often required.

In other words, _Nextflow_ provides an abstraction between the pipeline's functional logic and the underlying execution system. Thus it is possible to write a pipeline once and to seamlessly run it on your computer, a grid platform, or the cloud, without modifying it, by simply defining the target execution platform in the configuration file.

The following batch schedulers are supported:

-   [Open grid engine](http://gridscheduler.sourceforge.net/)
-   [Univa grid engine](http://www.univa.com/)
-   [Platform LSF](http://www.ibm.com/systems/technicalcomputing/platformcomputing/products/lsf/)
-   [Linux SLURM](https://computing.llnl.gov/linux/slurm/)
-   [PBS Works](http://www.pbsworks.com/gridengine/)
-   [Torque](http://www.adaptivecomputing.com/products/open-source/torque/)
-   [HTCondor](https://research.cs.wisc.edu/htcondor/)

The following cloud platforms are supported:

-   [Amazon Web Services (AWS)](https://aws.amazon.com/)
-   [Google Cloud Platform (GCP)](https://cloud.google.com/)
-   [Kubernetes](https://kubernetes.io/)

Read the `executor-page` to learn more about the Nextflow executors.
