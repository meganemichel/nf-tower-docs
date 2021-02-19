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

When you open [this link](https://portal.azure.com/#create/Microsoft.ResourceGroup) you'll notice the "Create new resource group" dialog, as shown below.

{{% pretty_screenshot img="/uploads/2021/02/azure_new_resource_group.png" %}}

<br>

1. Add the name for the resource group (for e.g. `towerrg`). 
2. Select the preferred region for this resource group. 
3. Click **Review and Create** to proceed to the review screen.
4. Click **Create** to create the resources.


### Storage account

The next step is to create the necessary Azure Storage. When you open [this link](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) you'll notice the "Create a storage account" dialog, as shown below.

{{% pretty_screenshot img="/uploads/2021/02/azure_create_storage_account.png" %}}

<br>

1. Add the name for the storage account (for e.g. `towerrgstorage`).
2. Select the preferred region for this resource group.
3. Click **Review and Create** to proceed to the review screen.
4. Click **Create** to create the resources.
5. Store the access keys for the newly created Azure Storage account as shown below.


{{% pretty_screenshot img="/uploads/2021/02/azure_storage_keys.png" %}}

### Batch account

The next step is to create the necessary Azure Storage. When you open [this link](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Batch%2FbatchAccounts) you'll notice the "Create a batch account" dialog, as shown below.

{{% pretty_screenshot img="/uploads/2021/02/azure_new_batch_account.png" %}}

<br>

1. Add the name for the storage account (for e.g. `towerrgbatch`).
2. Select the preferred region for this resource group.
3. Click **Review and Create** to proceed to the review screen.
4. Click **Create** to create the resources.
5. Store the access keys for the newly created Azure Batch account as shown below.


{{% pretty_screenshot img="/uploads/2021/02/azure_batch_keys.png" %}}



## Forge compute environment

{{% star "Congratulations!" %}}
You have completed the Azure environment setup for Tower.
{{% /star %}}


Now we can add a new **Azure Batch** environment in the Tower UI. To create a new compute environment, follow these steps:

**1.** In the navigation bar on the upper right, choose your account name then choose **Compute environments** and select **New Environment**.

{{% pretty_screenshot img="/uploads/2020/09/aws_new_env.png" %}}

</br>

**2.** Enter a descriptive name for this environment. For example, *Azure Batch (east-us)* and select **Azure Batch** as the target platform.

{{% pretty_screenshot img="/uploads/2021/02/azure_new_env_name.png" %}}

</br>

**3.** Add new credentials by selecting the **+** button. Choose a name, e.g. *Azure Credentials* and add the Access key and Secret key. These are the keys we saved in the previous steps when creating the Azure resources.

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

**7.** Enter the **VM type** e.g. `STANDARD_D2_V3`. This is the VM type which would be used as nodes for the Azure Batch pool.

**8.** Enter the **VM count** e.g. `3`. This is the maximum number of VMs sets the limit for Azure Batch autoscale.

**9.** Choose **Autoscale** to allow **Tower Forge** to expand and decrease the number of Virtual Machines during task execution.

**10.** Choose the **Dispose resources** option.

**Advanced options**

**11.** Select the job deletion criteria as one of the following options **On Success**, **Always** or **Never**.

**12.** Enable **Delete pools on completion** to delete the pool once the workflows execution is completed.

**13.** Optionally, specify the duration of the SAS token generated by Nextflow.

**14.** Select **Create** to finalize the compute environment setup. It will take approximately 60 seconds for all the resources to be created and then you will be ready to launch pipelines.

{{% pretty_screenshot img="/uploads/2021/02/azure_60s_new_env.png" %}}

<br>

{{% star "Amazing!" %}}
You now have everything you need to begin deploying massively scalable pipelines.
{{% /tip %}}

Jump to the documentation section for [Launching Pipelines](/docs/launch/overview/).


## Manual

This section is for users with a pre-configured Azure environment. You will need a Batch jobs pool, Batch account and a Storage account already set up.


## Manual compute environment
To create a new compute environment for Azure Batch (Manual):

**1.** Follow the guide for [Tower Forge](#forge) till step 5 and then selected the **Manual** option in the **Config mode**.

**2.** Add the name of the **Jobs pool**.

{{% pretty_screenshot img="/uploads/2021/02/azure_manual_jobs_pool.png" %}}

**3.** Select **Create** to finalize the compute environment setup.

{{% pretty_screenshot img="/uploads/2021/02/azure_env_creating.png" %}}


<br>

{{% star "Awesome!" %}}
You now have created a compute environment.
{{% /tip %}}

Jump to the documentation section for [Launching Pipelines](/docs/launch/overview/).
