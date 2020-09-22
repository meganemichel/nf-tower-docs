---
title: SLURM
weight: 2
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
{{% tip "Disclaimer" %}}
This guide assumes you already have an existing [GitLab account](https://gitlab.com/users/sign_in) and repository with a Jekyll or Hugo project. If you don't have an existing project, check out our [Quick start guide](/docs/quickstart/tour/), which contains guides and resources for building your first static site.
{{% /tip %}}

Forestry's allows you to import your static site through public and private GitLab repositories. This allows Forestry to sync any changes made by editors in Forestry to be comitted back to GitLab. This also allows developers to work on your website on their local machine, and have all changes by synced back to Forestry.

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
