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

## Tower Launch for AWS Batch
Nextflow Tower allows the seamless deployment of Nextflow data pipeles using AWS Batch computing service.

To enable such deployment create a AWS user account (or use an existing one) granting at least the following IAM permission:

AmazonS3ReadOnlyAccess
AmazonEC2ContainerRegistryReadOnly
CloudWatchLogsReadOnlyAccess
The following custom policy to grant the ability to submit and control Batch jobs.
Grant write access to any S3 bucket used a pipeline work directory using the following policy template.
You will also need to create the AWS Batch queue(s) required to deploy the Nextflow execution.

Tower can automate this configuration step, granting the required permissions to it as described at this link.


## Tower Forge for AWS Batch
Tower Forge automates the configuration of [AWS Batch](https://aws.amazon.com/batch/) compute environments and queues required for the deployment of Nextflow pipelines.

To enable this feature Tower requires the permissions listed in this [policy file](https://github.com/seqeralabs/nf-tower-aws/blob/master/forge/forge-policy.json).

Attach the policy to the AWS user account associated to your Tower configuration as described below:

Open the [AWS IAM console](https://console.aws.amazon.com/iam)
Select Users on the left menu
{{% pretty_screenshot img="/uploads/2020/09/aim_new_user.png" %}}
Choose (or create) the user associated in your Tower configuration. If you create a new user, choose the programmatic access type.
{{% pretty_screenshot img="/uploads/2020/09/create_policy.png" %}}
Select "Attach existing policies directly" box and click the "Create policy" button.
{{% pretty_screenshot img="/uploads/2020/09/review_policy.png" %}}
Choose JSON and copy the content of the [policy linked above](https://github.com/seqeralabs/nf-tower-aws/blob/master/forge/forge-policy.json).

Click on the button Review policy and confirm the operation clicking Create policy.

{{% tip "Policy" %}}
This policy also includes the minimal permissions required to allow the user to submit Batch jobs, gather containers execution metadata, read CloudWatch logs and access the S3 bucket in your AWS account in read-only mode.
{{% /tip %}}

{{% warning %}}
You may need to further customized the IAM permissions to access private ECR registries, write to S3 buckets or access other AWS resources.
{{% /warning %}}

## Create a new compute environment
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
