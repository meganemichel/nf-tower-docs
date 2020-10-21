---
title: Tower webhooks
menu:
  docs:
    parent: Webhooks
    weight: 2
---

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
