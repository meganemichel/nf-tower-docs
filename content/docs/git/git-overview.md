---
title: Git Overview
aliases:
- "/docs/git-overview"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"
headline: "Leveraging git version control for workflows"
description: 'Managing and connecting to workflow git repositories'

menu:
  docs:
    parent: Git Integration
    weight: 1
---
One thing that makes Nextflow different from other workflow managers is the built-in support for [Git](https://git-scm.com).

The idea behind is to allow the handling of complex pipeline projects as Git projects.

Pipelines may be composed by many assets (source code scripts, deployment settings, dependency descriptors such as Conda or Docker files, etc.).

As as a single Git project, they can be precisely tracked and deployed specifying a Git tag, release or commit id.

This along with the support for containerization, is key for **enabling replicable pipeline executions**.

It also provides the ability to continuously test and validate your pipeline as code evolves over time.

The following sections document how to connect to execute pipelines hosted in a public git repository and how to connect to private git repositories:

  * [Executing a pipeline hosted in a public git repository](/docs/git/git-public/)
  * [Connecting to a private git repository](/docs/git/git-private/)
