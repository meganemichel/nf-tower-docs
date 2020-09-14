---
title: Version control
aliases:
- "/docs/git-intergration"
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
    parent: Git Integration
    weight: 1
---

## Git integration
One thing that makes Nextflow different from other workflow managers is the built-in support for the Git source code management system. 

The idea behind is to allow the handling of complex pipeline projects as Git projects.

Pipelines may be composed by many assets (source code scripts, deployment settings, dependency descriptors such as Conda or Docker files, etc.).

As as a single Git project, they can be precisely tracked and deployed specifying a Git tag, release or commit id. 

This along with the support for containerization, is key for enabling replicable pipeline executions. 

It also provides the ability to continuously test and validate your pipeline as code evolves over time.

## Publically-hosted Git platforms

As Nextflow data pipelines are simply Git repositories, these can be either public or private and hosted on any of the popular GitHub, GitLab or BitBucket platform. 
 
Credentials for private repositories can be managed from the credentials page.

## Private-hosted Git platforms

It is also possible to specify Git server endpoints for private hosting.

These can be specified in a file `tower.yaml` and must be accessible from the backend and cron container instances. 

```yaml
tower:
  scm:
    providers:
      my_org_bitbucket:
        server: "https://bitbucket.my-org.com"
        endpoint: "<the API endpoint URL if different from the above>"
        platform: bitbucketserver
        user: some_user_name
        password: password_or_access_token
```

{{% tip %}}
This [Git access options](https://www.nextflow.io/docs/latest/sharing.html#scm-configuration-file Nextflow SCM configuration file) used by Nextflow however using YAML format. You can first validate your provder with a Nextflow execution using the standard SCM file and then convert it to the YAML structure show above for Tower.

{{% /tip %}}