---
title: AWS Batch
weight: 1
publishdate: 2017-12-31T04:00:00.000+00:00
expirydate: 2030-01-01T04:00:00.000+00:00
date: '2017-04-06T04:00:00.000+00:00'
layout: single
images:
- "/uploads/2018/01/OGimage-01-docs-3x.jpg"
menu:
  docs:
    parent: Compute Environments
    weight: 2

---
{{% tip "Disclaimer" %}}
This guide assumes you already have an existing [AWS Account](https://aws.amazon.com/), you have access to your [AWS Access and Secret Keys](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html), and you have created an S3 bucket to store your workflow results.
{{% /tip %}}

To create a new compute environment for AWS, follow these steps:

{{% pretty_screenshot img="/uploads/2020/09/new_env.png" %}}

In the navigation bar on the upper right, choose your account name then choose "Compute environments". Click on the *New Environment* button.

{{% pretty_screenshot img="/uploads/2020/09/new_env_name.png" %}}
Choose a descriptive name for this environment. For example "AWS Batch Spot (eu-west-1)" and Select *Amazon Batch* as the target platform

{{% pretty_screenshot img="/uploads/2020/09/aws_keys.png" %}}

Add new credentials by clicking the the "+" button. Choose a name, add the Access key and Secret key.

{{% tip "Multiple credentials" %}}
Note You can create multiple credentials. These will be available from the dropdown menu.
{{% /tip %}}

{{% pretty_screenshot img="/uploads/2020/09/aws_keys.png" %}}

Select a region, for example "eu-west-1 - Europe (Ireland)", and choose an S3 bucket path, For example "s3://tower-genomic-pipelines".

{{% warning %}}
The bucket should be in the same region you selected above!
{{% /warning %}}

{{% pretty_screenshot img="/uploads/2020/09/cpus.png" %}}

Type 64 in Max CPUs. You can leave the other options with their default values and click on the "Create" button to finalize the creation of your new AWS environment.

{{% pretty_screenshot img="/uploads/2020/09/60s_new_env.png" %}}

It will take approximately 60 seconds for the resources to be created. After this, the compute environment will be ready to launch pipelines.
