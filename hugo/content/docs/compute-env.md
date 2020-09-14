---
title: Configuration
aliases:
- "/docs/compute-environments"
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
    parent: Compute Environments
    weight: 1
---


## Overview
Tower uses a concept of a compute environments to define the target execution platform for the workflow. 

## AWS Batch
To create a new compute environment for AWS, the user will:

1. Navigate to "Compute environments" to view the existing environments.
2. Select "New Environment"
3. Choose a descriptive name for this environment. For example "AWS Batch Spot (eu-west-1)"
4. Select a target platform. For example "Amazon Batch".
5. Add credentials from either the dropdown of the "+" button.
6. If adding new credentials for AWS, choose a name, add the Access key and Secret key. 
7. Select a region. For example "eu-west-1 - Europe (Ireland"
8. Choose an S3 bucket path. This bucket should be in the same region as above. For example ("s3://nextflow-ci").
9. Type 64 in Max CPUs.
10. Select "Create" to finalise the creation of the resources.

This will take approximately 60 seconds for the resources to be created. After this, the compute environment will be ready to launch pipelines.

## Google Cloud Lifesciences
To create a new compute environment for AWS, the user will:

1. Navigate to "Compute environments" to view the existing environments.
2. Select "New Environment"
3. Choose a descriptive name for this environment. For example "AWS Batch Spot (eu-west-1)"
4. Select a target platform. For example "Amazon Batch".
5. Add credentials from either the dropdown of the "+" button.
6. If adding new credentials for AWS, choose a name, add the Access key and Secret key. 
7. Select a region. For example "eu-west-1 - Europe (Ireland"
8. Choose an S3 bucket path. This bucket should be in the same region as above. For example ("s3://nextflow-ci").
9. Type 64 in Max CPUs.
10. Select "Create" to finalise the creation of the resources.

This will take approximately 60 seconds for the resources to be created. After this, the compute environment will be ready to launch pipelines.

## Slurm

## LSF

