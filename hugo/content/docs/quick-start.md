---
title: Getting Started
aliases:
- "/docs/quick-start"
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
    parent: Quick Start
    weight: 3
  main:
    name: Docs
    weight: 3
---

## Overview

Tower enables you to run Nextflow pipelines at scale in cloud or on-premise compute environments.

This tour will walk you how to run a basic RNASeq pipeline and set up your own compute environment in your AWS account.

***

## First pipeline

[Sign in](https://tower.nf) to Tower, and then navigate to the [Launch] dialog box in the menu navigation bar.

1. Select the default [compute environment](/compute-evironment) Tower (Kubernetes)
2. Select a Nextflow pipeline to launch from the list of options. _https://github.com/nextflow-io/rnaseq-nf is the default_.
3. Select "Launch" to execute the pipeline.

Congrats, your first RNASeq pipeline is now running in the cloud!

You can [monitor](/monitor) the progress of the pipeline, see the details of the individual tasks as they are executed and visualise the resources used by the various processes.

***

## Next Steps