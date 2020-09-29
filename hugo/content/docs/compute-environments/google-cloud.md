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

Next, create a bucket in the project (make sure you the right project is selected on top of the page left to the search bar). Click on the Navigation menu and click on **Storage** and the on **Create Bucket**.

{{% pretty_screenshot img="/uploads/2020/09/google_name_bucket.png" %}}

{{% warning %}}
DO NOT USE underscores in your bucket name. Join words with hyphens instead.   
{{% /warning %}}


{{% pretty_screenshot img="/uploads/2020/09/google_storage.png" %}}

{{% warning %}}
Google Life Sciences is available in limited number of [locations](https://cloud.google.com/life-sciences/docs/concepts/locations). It's probably best to choose your bucket location in one of these (e.g europe-west2 (London)).
{{% /warning %}}

{{% pretty_screenshot img="/uploads/2020/09/google_location.png" %}}

Select the **location type** to Region and the **Location** to one of the regions shown bellow. Note this list can evolve, the latest list is available [here](https://cloud.google.com/life-sciences/docs/concepts/locations)

{{% pretty_screenshot img="/uploads/2020/09/google_life_sciences_regions.png" %}}

Next you need to download credentials to give Tower access to your Google project.

{{% pretty_screenshot img="/uploads/2020/09/google_select_service_accounts.png" %}}

In the Navigation bar, Click **IAM** and **Service Account**

{{% pretty_screenshot img="/uploads/2020/09/google_service_account_name.png" %}}

Click **+ CREATE SERVICE ACCOUNT**. Choose a name and a description.

{{% pretty_screenshot img="/uploads/2020/09/google_service_account_role.png" %}}

Set the role to Editor, leave the **Grant users access to this service account (optional)** options empty and click **CREATE**.

{{% pretty_screenshot img="/uploads/2020/09/google_create_key.png" %}}

On the newly created **Service ccount** click **Actions** and **Create key**.

{{% pretty_screenshot img="/uploads/2020/09/google_key_json.png" %}}

Select the JSON format and click **CREATE**. This will download a JSON file with your credentials.


Now, we have created a project, enabled Google Life Sciences API, created a bucket and a JSON file containing our credentials, we are ready to set up a new environment en Tower.

## Create a new Google Life Sciences compute environment

To create a new compute environment for AWS, follow these steps:

{{% pretty_screenshot img="/uploads/2020/09/new_env.png" %}}

In the navigation bar on the upper right, choose your account name then choose "Compute environments". Click on the *New Environment* button.

{{% pretty_screenshot img="/uploads/2020/09/google_new_env.png" %}}

Choose a descriptive name for this environment. For example "Google (europe-west2)" and Select **Google Life Sciences** as the target platform

{{% pretty_screenshot img="/uploads/2020/09/google_tower_credentials.png" %}}

Click the **+** sign to add new Credentials. Name your credentials and copy-paste the contents from the JSON file downloaded in the previous section.

{{% pretty_screenshot img="/uploads/2020/09/google_tower_location.png" %}}

Select the same **Region** we used for the bucket in the previous section (e.g Europe-west2). You can leave the **Location** empty (google will run the life sciences service where your bucket is located). The **Pipeline work directory** should be your bucket path e.g *gs://nf-tower-bucket*. You can leave the other options empty and click **Create**.
