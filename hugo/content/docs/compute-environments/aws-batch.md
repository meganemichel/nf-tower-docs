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
<!-- If you already have Batch environment pre-configured skip Froge and go to Launch -->
This guide assumes you already have an existing [AWS Account](https://aws.amazon.com/).
{{% /tip %}}

There are two ways of running AWS Batch workflows with Tower. An automated approach **Tower Froge** in which Tower will automatically create AWS queues for your workflows, and a manual approach **Tower Launch** where you can tell Tower which queues to use.

If you don't yet have a AWS Batch environment fully set-up follow the [Tower forge guide](#tower-forge-for-aws-batch)

If you have been allocated AWS queues in your AWS account follow the [Tower launch section](#tower-launch-for-aws-batch)

## Tower Forge for AWS Batch

<!-- Add explenation for what is Forge and disclaimer -->
{{% warning %}}
You need to follow these instructions if you have not pre-configured an AWS batch environment. If you had please skip this section and jump to the [Tower launch section](#tower-launch-for-aws-batch)
{{% /warning %}}

Tower Forge automates the configuration of [AWS Batch](https://aws.amazon.com/batch/) compute environments and queues required for the deployment of Nextflow pipelines.

To enable this feature Tower requires the permissions listed in this [policy file](https://github.com/seqeralabs/nf-tower-aws/blob/master/forge/forge-policy.json).

The steps bellow will guide you to create a new user and attach a policy to the newly created user:

Open the [AWS IAM console](https://console.aws.amazon.com/iam), select Users on the left menu and click the *Add User* button on top.

{{% pretty_screenshot img="/uploads/2020/09/aws_aim_new_user.png" %}}

Name your user and choose the **programmatic access** type. Then click on the **next: Permissions** button.
In step 2, 3 and 4 click on the **Next: Tags** button, **Next: Review** and **Create User**.
Note this user has not been given permissions.

{{% pretty_screenshot img="/uploads/2020/09/aws_user_created.png" %}}

Save your keys, we will need the access key and the secret key in the [next section](#create-a-new-compute-environment). Press the **Close** button.

{{% pretty_screenshot img="/uploads/2020/09/aws_add_inline_policy.png" %}}

Back in the users table, click on the newly created user and click on **+ add inline policy**.

{{% pretty_screenshot img="/uploads/2020/09/aws_review_policy.png" %}}

Choose JSON and copy the content of the [policy linked above](https://github.com/seqeralabs/nf-tower-aws/blob/master/forge/forge-policy.json).

Click on the **Review policy** button, name your policy, and confirm the operation clicking on the **Create policy** button. Your user should now have attached inline policy.

{{% tip "Policy" %}}
This policy also includes the minimal permissions required to allow the user to submit Batch jobs, gather containers execution metadata, read CloudWatch logs and access the S3 bucket in your AWS account in read-only mode.
{{% /tip %}}

Next we need to create an **S3 Bucket** to access files and store results and grant our new user access to this bucket. In AWS navigate to the S3 service and select **Create New Bucket**

{{% pretty_screenshot img="/uploads/2020/09/aws_create_bucket.png" %}}

Name your Bucket and select the region. Note the region of the bucket needs to be in the same region as the compute environment we will set in the next session.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_bucket_configure_options.png" %}}

{{% pretty_screenshot img="/uploads/2020/09/aws_new_bucket_set_permissions.png" %}}

{{% pretty_screenshot img="/uploads/2020/09/aws_new_bucket_review.png" %}}

Choose the default options and create your new bucket.

Now that we have created an S3 Bucket we need to set a policy for our user. Go back to the users table in the [AIM service](https://console.aws.amazon.com/iam/home), and click on the user previously created.

{{% pretty_screenshot img="/uploads/2020/09/aws_user_s3_inline_policy.png" %}}

 click on **+ add inline policy**.

{{% pretty_screenshot img="/uploads/2020/09/aws_s3_policy.png" %}}

 Choose JSON and copy the content of [this policy](https://github.com/seqeralabs/nf-tower-aws/blob/master/launch/s3-bucket-write.json). Replace line 10 and 21 with the bucket name you previously created.

 {{% pretty_screenshot img="/uploads/2020/09/aws_name_policy.png" %}}

Name your policy and create it.

Now that **Tower forge** has been set up we can add a new **AWS Batch Forge** environment in the [Nextflow Tower UI](#create-a-new-aws-batch-forge-compute-environment)

## Create a new AWS Batch Forge compute environment
To create a new compute environment for AWS, follow these steps:

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env.png" %}}

In the navigation bar on the upper right, choose your account name then choose "Compute environments".
Click on the **New Environment** button.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env_name.png" %}}

Choose a descriptive name for this environment.
For example "AWS Batch Spot (eu-west-1)" and Select **Amazon Batch** as the target platform

{{% pretty_screenshot img="/uploads/2020/09/aws_keys.png" %}}

Add new credentials by clicking the the "+" button. Choose a name, add the Access key and Secret key. These are the keys we saved in the previous step when creating the new user in AWS.

{{% tip "Multiple credentials" %}}

Note You can create multiple credentials. These will be available from the dropdown menu.
{{% /tip %}}

{{% pretty_screenshot img="/uploads/2020/09/aws_s3_bucket_region.png" %}}

Select a region, for example "eu-west-1 - Europe (Ireland)", and choose an S3 bucket we created in the previous section e.g: "s3://tower-bucket". Choose **Batch Froge** as config mode.

{{% warning %}}
The bucket should be in the same region you selected above!
{{% /warning %}}

{{% pretty_screenshot img="/uploads/2020/09/aws_cpus.png" %}}

Type 64 in Max CPUs. You can leave the other options with their default values and click on the "Create" button to finalize the creation of your new AWS environment. The **Allowed S3 buckets** are additional buckets your workflows might need to access input data or to write output files. The bucket in the **pipeline work directory** is added by default to the **Allowed S3 buckets**.

{{% pretty_screenshot img="/uploads/2020/09/aws_60s_new_env.png" %}}

It will take approximately 60 seconds for the resources to be created. After this, the compute environment will be ready to launch pipelines.


## Tower Launch for AWS Batch

<!-- Add explenation for what is Launch and disclaimer -->
Follow this guide if your AWS environment is already setup and you have been allocated AWS queues for you jobs.

To enable Tower in your existing deployment you need granting at least the following IAM permission:

- `AmazonS3ReadOnlyAccess`
- `AmazonEC2ContainerRegistryReadOnly`
- `CloudWatchLogsReadOnlyAccess`
- The following [custom policy](launch-policy.json) to grant the ability to submit and control Batch jobs.
- Grant write access to any S3 bucket used a pipeline work directory using the following [policy template](s3-bucket-write.json).

Now that **Tower Launch** has been set up we can add a new **AWS Batch Launch** environment in the [Nextflow Tower UI](#create-a-new-aws-batch-launch-compute-environment)

## Create a new AWS Batch Launch compute environment
To create a new compute environment for AWS, follow these steps:

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env.png" %}}

In the navigation bar on the upper right, choose your account name then choose "Compute environments". Click on the **New Environment** button.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_launch_env.png" %}}

Choose a descriptive name for this environment. For example "AWS Batch Launch (eu-west-1)" and Select **Amazon Batch** as the target platform

{{% pretty_screenshot img="/uploads/2020/09/aws_keys.png" %}}

Add new credentials by clicking the the "+" button. Choose a name, add the **Access key** and **Secret key** from your AIM user.

{{% tip "Multiple credentials" %}}
Note You can create multiple credentials. These will be available from the dropdown menu.
{{% /tip %}}


{{% pretty_screenshot img="/uploads/2020/09/aws_new_env_manual_config.png" %}}

Select a region, for example "eu-west-1 - Europe (Ireland)", and choose an S3 bucket path, For example "s3://tower-bucket" and choose the **manual** config mode. Select a Head queue (this is where the Nextflow application will run) and a compute queue (where Nexflow will submit job executions), and click **create**.

It will take approximately 60 seconds for the resources to be created. After this, the compute environment will be ready to launch pipelines.
