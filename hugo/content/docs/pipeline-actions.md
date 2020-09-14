---
title: Automations
aliases:
- "/docs/pipeline-actions"
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
    parent: Pipeline Actions
    weight: 1
---

## Overview

Pipeline Actions allows for users to associate a Nextflow pipeline project with an action event and define the launch parameters for that action is triggered. 

In this way, pipelines execution can be triggered automatically based on events.

The definition of the launch parameters is the same as the "Launch" dialog above

Currently there are two actions which can be used to trigger the execution of a pipeline.

* GitHub Webhook
* Tower Launch Hook

## GitHub Webhooks

This pipeline action links projects hosted in a GitHub repository to a Tower Launch deployment environment, in such a way that every time a code change is pushed to the Git repository, the pipeline execution is launched by Nextflow Tower.

## Tower Launch Hook
There are however situations where users may wish to trigger pipeline executions independently from any change to pipeline code. 

When creating a Pipeline Action, users can also select Tower launch hook as the event source. 

When doing so, Tower creates a custom endpoint URL which can be used to trigger the execution of your pipeline programmatically from any script or even other web services.
