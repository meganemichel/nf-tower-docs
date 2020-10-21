---
title: GitHub webhooks
menu:
  docs:
    parent: Webhooks
    weight: 1
---

Webhooks allow you to Launch pipelines automatically based on events. Tower currently offers support for native GitHub webhook and a general Tower webhook that can be invoked programatically. Support for native Bitbucket and Gitlab webhooks will be supported soon.

A **Pipeline action** listens to changes in the pipeline repository, when a change occurs Tower trigger the launch of the pipeline as a response.

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
<br>
**[NEXT Tower webhooks →](/docs/webhooks/tower/)**
