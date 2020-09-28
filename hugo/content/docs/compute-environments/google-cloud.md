---
title: Google Cloud
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
    weight: 3

---
---
{{% tip "Disclaimer" %}}
<!-- If you already have Batch environment pre-configured skip Froge and go to Launch -->
This guide assumes you already have an existing [Google Cloud Account](https://console.cloud.google.com). The first section guides you through setting up your Google Cloud account and enable Google Life Sciences API. The second section guides you through creating a new Google Cloud environment in Tower.
{{% /tip %}}

## Setting up Google Cloud

First Step is to create a a Google Cloud project. Navigate to the [Google Project Selector page](https://console.cloud.google.com/projectselector2) and select **CREATE PROJECT**

{{% pretty_screenshot img="/uploads/2020/09/google_create_project.png" %}}

Choose a name for your project e.g: "tower-nf", the organisation and location will be set by default to your current account setup.

Before enabling the **Google Life Sciences** API make sure that billing is enabled for the project you just created. In the project page select the Navigation menu from the top left of the page and select **Billing**. You can follow the instructions [here](https://cloud.google.com/billing/docs/how-to/modify-project).

To enable **Google Life Sciences API** open this [link](https://console.cloud.google.com/flows/enableapi?apiid=lifesciences.googleapis.com%2Ccompute.googleapis.com%2Cstorage-api.googleapis.com), select the project we just created and click **Continue**.

Next, create a bucket in the project (make sure you the right project is selected on top of the page left to the search bar). Click on the Navigation menu and click on **Storage** and the on **Create Bucket**

{{% pretty_screenshot img="/uploads/2020/09/google_storage.png" %}}

{{% warning %}}
Google Life Sciences is available in limited number of [locations](https://cloud.google.com/life-sciences/docs/concepts/locations). It's probably best to choose your bucket location in one of these (e.g europe-west2 (London)).
{{% /warning %}}

{{% pretty_screenshot img="/uploads/2020/09/google_location.png" %}}

Select the **location type** to Region and the **Location** to one of the regions shown bellow. Note this list can evolve, the latest list is available [here](https://cloud.google.com/life-sciences/docs/concepts/locations)

{{% pretty_screenshot img="/uploads/2020/09/google_life_sciences_regions.png" %}}

Now, we have created a project, enabled Google Life Sciences API and created a bucket we are ready to set up a new environment en Tower.

## Create a new Google Life Sciences compute environment

To create a new compute environment for AWS, follow these steps:

{{% pretty_screenshot img="/uploads/2020/09/new_env.png" %}}

In the navigation bar on the upper right, choose your account name then choose "Compute environments". Click on the *New Environment* button.

{{% pretty_screenshot img="/uploads/2020/09/new_launch_env.png" %}}

Choose a descriptive name for this environment. For example "AWS Batch Launch (eu-west-1)" and Select *Amazon Batch* as the target platform

{{% pretty_screenshot img="/uploads/2020/09/aws_keys.png" %}}

Add new credentials by clicking the the "+" button. Choose a name, add the **Access key** and **Secret key** from your AIM user.

{{% tip "Multiple credentials" %}}
Note You can create multiple credentials. These will be available from the dropdown menu.
{{% /tip %}}


{{% pretty_screenshot img="/uploads/2020/09/new_env_manual_config.png" %}}


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
