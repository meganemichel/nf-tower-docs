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

With **Pipeline actions**, launching pipelines can be triggered automatically based on events. Tower currently supports two actions which can be used to trigger the execution of a pipeline.

* GitHub Webhook
* Tower Launch Hook

## GitHub web hooks

Users can setup a **Pipeline action** to listen if changes in the pipeline repository occur and trigger the launch of the pipeline as a response.

To create a new **Pipeline action** navigate to the user menu on the top right, select **Pipeline actions** and create a new action.

{{% pretty_screenshot img="/uploads/2020/10/actions_new.png" %}}

**1.** Choose a name for your Action.

**2.** Choose the Github web hook as event source.

{{% pretty_screenshot img="/uploads/2020/10/actions_githook.png" %}}

**3.** Choose the environment where the pipeline will be executed

**4.** Choose the pipeline to launch and optionally the revision numbers

{{% star %}}
The web hook will listen to the selected repository and revision. When a new commit occurs for the selected revision an event will be triggered in Tower and the pipeline for the selected revision will be launched.
{{% /star %}}

**5.** Choose the working directory, the config profiles, and parameters. Click **Create**

{{% pretty_screenshot img="/uploads/2020/10/actions_params.png" %}}

{{% star %}}

You now have an active **Pipeline action** always listening to the latest changes in your repository.

{{% /star %}}

{{% pretty_screenshot img="/uploads/2020/10/actions_created.png" %}}

## Tower launch hook

There are however situations where users may wish to trigger pipeline executions independently from any change to pipeline code, for instance updates in an input file or adding samples to a directory. We can create a **Tower launch hook** which will create a custom endpoint URL which can be used to trigger the execution of your pipeline programmatically from any script or even other web services.

**1.** After naming your Action, select **Tower launch hook** as event source.

{{% pretty_screenshot img="/uploads/2020/10/actions_tower_hook.png" %}}

**2.** As in the previous section, select the **environment** to execute your pipeline, the **pipeline repository URL**, the **Revision number**, the **Work directory**, the **Config profiles**, and the **Pipeline parameters**, and click **Create**.

{{% pretty_screenshot img="/uploads/2020/10/actions_tower_hook_params.png" %}}

**Tower launch hook** has created an endpoint that can be used to programatically launch the corresponding pipeline. the example snippet bellow shows a `cURL` command with the authentication token.  

{{% pretty_screenshot img="/uploads/2020/10/actions_endpoint.png" %}}

{{% star %}}

You now have created an endpoint to programatically launch a specific pipeline.

{{% /star %}}

{{% tip %}}
When you create a **Tower launch hook** you also create an access Token to allow submitting executions to Tower.
{{% /tip %}}

{{% pretty_screenshot img="/uploads/2020/10/actions_new_token.png" %}}

Access Tokens are accessible on the [tokens page](https://tower.nf/tokens) and can also be accessed from the navigation menu.

{{% pretty_screenshot img="/uploads/2020/10/actions_access_tokens.png" %}}
