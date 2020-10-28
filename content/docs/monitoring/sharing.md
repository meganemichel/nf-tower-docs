---
title: Sharing pipelines
aliases:
- "/docs/teams/sharing"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"
headline: "How to share pipelines with collaborators"
description: 'Sharing Nextflow pipelines'

menu:
  docs:
    parent: Monitoring Pipelines
    weight: 7
---

{{% warning %}}
To ensure your collaborators have access to your pipelines' results, make sure to give them access to the compute environment where the shared pipeline is being executed. For the moment you have to do this outside of Tower: e.g in AWS, Google, or in your on premise cluster.
{{% /warning %}}

To share a pipeline with a collaborator click on the `Sharing` icon.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_sharing1.png" %}}

Click on the `Add Collaborator` button, add your collaborator's email and click `Confirm`.

{{% pretty_screenshot img="/uploads/2020/10/monitoring_sharing2.png" %}}

An email with the pipeline URL will be sent

{{% pretty_screenshot img="/uploads/2020/10/monitoring_sharing3.png" %}}


{{% warning %}}
Your collaborator's account email must match the email where you sent the invite.
{{% /warning %}}
