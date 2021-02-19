---
title: Azure Batch
weight: 1
layout: single
publishdate: 2021-02-19 04:00:00 +0000
authors:
  - "Abhinav Sharma"
  - "Seqera Labs"

headline: 'Azure Batch Compute Environment'
description: 'Step-by-step instructions to set up Azure Batch in Nextflow Tower.'
menu:
  docs:
    parent: Compute Environments
    weight: 2

---
## Overview
{{% tip "Disclaimer" %}}
<!-- If you already have Batch environment pre-configured, skip Forge and go to Launch -->
This guide assumes you have an existing [Azure Account](https://azure.microsoft.com/en-us). Sign up for a free Azure account [here](https://azure.microsoft.com/en-us/free/).
{{% /tip %}}

There are two ways to create a **Compute Environment** for **Azure Batch** with Tower.

1. **Tower Forge** for Azure Batch automatically creates Azure Batch resources in your Azure account.

2. **Tower Launch** allows you to create a compute environment using existing Azure Batch resources.

If you don't yet have an Azure Batch environment fully set-up, following the [Tower Forge](#forge) guide is suggested. If you have been provided with an Azure Batch queue from your account administrator, or if you have set up Azure Batch previously, follow the [Tower Launch](#manual) guide.

## Forge

<!-- Add explanation for what is Forge and disclaimer -->
{{% warning %}}
Follow these instructions if you have not pre-configured an Azure Batch environment. Note that this will create resources in your Azure account that you may be charged for by Azure.
{{% /warning %}}

Tower Forge automates the configuration of an [Azure Batch](https://azure.microsoft.com/en-us/services/batch/) compute environment and queues required for the deployment of Nextflow pipelines.

## Forge Azure Resources

### Resource group

To create the necessary Azure Batch and Azure Storage accounts, we must first create a **Resource Group** in the region of your choice.

When you open [this link](https://portal.azure.com/#create/Microsoft.ResourceGroup) you'll notice the "Create new Resource Group" dialog, as shown below.

{{% pretty_screenshot img="/uploads/2021/02/azure_new_resource_group.png" %}}

<br>

1. Add the name for the resource group (for e.g. `towerrg`). 
2. Select the preferred region for this resource group. 
3. Click **Review and Create** to proceed to the review screen.
4. Click **Create** to create the resources.


### Storage account

The next step is to create the necessary Azure Storage. When you open [this link](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) you'll notice the "Create new Storage Account" dialog, as shown below.

{{% pretty_screenshot img="/uploads/2021/02/azure_create_storage_account.png" %}}

<br>

1. Add the name for the storage account (for e.g. `towerrgstorage`).
2. Select the preferred region for this resource group.
3. Click **Review and Create** to proceed to the review screen.
4. Click **Create** to create the resources.
5. TODO: Store the access keys

### Batch account

The next step is to create the necessary Azure Storage. When you open [this link](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Batch%2FbatchAccounts) you'll notice the "Create new Storage Account" dialog, as shown below.

{{% pretty_screenshot img="/uploads/2021/02/azure_new_batch_account.png" %}}

<br>

1. Add the name for the storage account (for e.g. `towerrgbatch`).
2. Select the preferred region for this resource group.
3. Click **Review and Create** to proceed to the review screen.
4. Click **Create** to create the resources.
5. TODO: Store the access keys


## Forge compute environment

{{% star "Congratulations!" %}}
You have completed the Azure environment setup for Tower.
{{% /star %}}


Now we can add a new **Azure Batch** environment in the Tower UI. To create a new compute environment, follow these steps:

**1.** In the navigation bar on the upper right, choose your account name then choose **Compute environments** and select **New Environment**.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env.png" %}}

</br>

**2.** Enter a descriptive name for this environment. For example, *Azure Batch Spot (eu-west-1)* and select **Amazon Batch** as the target platform.

{{% pretty_screenshot img="/uploads/2021/02/azure_new_env_name.png" %}}

</br>

**3.** Add new credentials by selecting the **+** button. Choose a name, e.g. *Azure Credentials* and add the Access key and Secret key. These are the keys we saved in the previous steps when creating the Azure IAM user.

{{% pretty_screenshot img="/uploads/2021/02/azure_keys.png" %}}

<br>

{{% tip "Multiple credentials" %}}
You can create multiple credentials in your Tower environment.
{{% /tip %}}

<br>

**4.** Select a **Region**. For example *eu-west-1 - Europe (Ireland)*, and in the **Pipeline work directory** enter the S3 bucket we created in the previous section e.g: `s3://unique-tower-bucket`.


{{% pretty_screenshot img="/uploads/2021/02/azure_blob_container_region.png" %}}

{{% warning %}}
The bucket should be in the same **Region** as selected above.
{{% /warning %}}

**5.** Select **Batch Forge** as the **Config Mode**.


<br>
{{% pretty_screenshot img="/uploads/2021/02/azure_config_mode_forge.png" %}}
<br>

**7.** Enter the **Max CPUs** e.g. `64`. This is the maximum number of combined CPUs (the sum of all instances CPUs) Azure Batch will launch at any time.

**8.** Choose **Auto scale** to allow **Tower Forge** to expand the number of Virtual Machines during task execution.

**13.** Choose the **Dispose resources** option.

**Advanced options**

{{% warning "AMI ID - AMI requirements for Azure Batch use" %}}

To use an existing AMI, make sure the AMI is based on an Amazon Linux-2 ECS optimized image that meets the Batch requirements. To learn more about approved versions of the Amazon ECS optimized AMI, visit this [link](https://docs.Azure.amazon.com/batch/latest/userguide/compute_resource_AMIs.html#batch-ami-spec)

{{% /warning %}}

{{% warning "Min CPUs - Editing this will result in additional Azure costs" %}}

 Keeping EC2 instances running may result in additional costs. You will be billed for these running EC2 instances regardless of whether you are executing pipelines or not.

{{% /warning %}}

Note that if **Min CPUs** is greater than `0`, EC2 instances will remain active. An advantage of this is that a pipeline execution will initialize faster but it may have additional costs.

{{% pretty_screenshot img="/uploads/2021/01/Azure_warning_min_cpus.png" %}}

<br>

**14.** Select **Create** to finalize the compute environment setup. It will take approximately 60 seconds for all the resources to be created and then you will be ready to launch pipelines.

{{% pretty_screenshot img="/uploads/2021/02/azure_60s_new_env.png" %}}

<br>

{{% star "Amazing!" %}}
You now have everything you need to begin deploying massively scalable pipelines.
{{% /tip %}}

Jump to the documentation section for [Launching Pipelines](/docs/launch/overview/).


## Manual

This section is for users with a pre-configured Azure environment. You will need a Batch queue, Batch compute environment, an IAM user and an S3 bucket already set up.


## Manual compute environment
To create a new compute environment for Azure Batch (Manual):

**1.** In the navigation bar on the upper right, choose your account name then choose **Compute environments** and select on **New Environment**.

{{% pretty_screenshot img="/uploads/2020/09/Azure_new_env.png" %}}

<br>

**2.** Choose a descriptive name for this environment. For example "Azure Batch Launch (eu-west-1)" and Select **Amazon Batch** as the target platform

{{% pretty_screenshot img="/uploads/2020/09/Azure_new_launch_env.png" %}}

<br>

**3.** Add new credentials by clicking the "+" button. Choose a name, add the **Access key** and **Secret key** from your IAM user.

{{% pretty_screenshot img="/uploads/2020/09/Azure_keys.png" %}}

{{% tip "Multiple credentials" %}}
You can create multiple credentials in your Tower environment. See the section **Credentials Management**.
{{% /tip %}}

<br>

**4.** Select a region, for example "eu-west-1 - Europe (Ireland)"

**5.** Enter an S3 bucket path, For example "s3://tower-bucket"

**6.** the **Manual** config mode.

**7.** Enter the **Head queue** which is the name of the Azure Batch queue that the Nextflow driver job will run and a **Compute queue** which is the name of the Azure Batch queue that tasks will be submitted to.

**8.** Select **Create** to finalize the compute environment setup.

{{% pretty_screenshot img="/uploads/2020/09/Azure_new_env_manual_config.png" %}}

<br>

{{% star "Awesome!" %}}
You now have created a compute environment.
{{% /tip %}}

Jump to the documentation section for [Launching Pipelines](/docs/launch/overview/).
