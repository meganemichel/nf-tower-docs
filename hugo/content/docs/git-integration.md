---
title: Git Integration
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
    parent: Version control
    weight: 1
---

One thing that makes Nextflow different from other workflow managers is the built-in support for [Git](https://git-scm.com).

The idea behind is to allow the handling of complex pipeline projects as Git projects.

Pipelines may be composed by many assets (source code scripts, deployment settings, dependency descriptors such as Conda or Docker files, etc.).

As as a single Git project, they can be precisely tracked and deployed specifying a Git tag, release or commit id.

This along with the support for containerization, is key for **enabling replicable pipeline executions**.

It also provides the ability to continuously test and validate your pipeline as code evolves over time.

## Public Git repositories

As Nextflow data pipelines are simply Git repositories, these can be either public or private and hosted on any of the popular GitHub, GitLab or BitBucket platform.

Launching public hosted git pipelines simply requires adding the git repo URL in the **pipeline to launch** field. Note the revision numbers for the given repository are automatically loaded. By default, the stable `master` branch will be executed.

{{% pretty_screenshot img="/uploads/2020/10/git_public_repo.png" %}}

{{% tip %}}
[nf-core](https://nf-co.re/pipelines) is a great resource for public nextflow pipelines.
{{% /tip %}}


## Private Git repositories

{{% warning %}}
All credentials are securely stored using advanced encryption (AES-256) and never exposed by any Tower API.
{{% /warning %}}

Credentials for private git repositories can be managed from the [credentials page](https://tower.nf/credentials) accessible on the right top menu, under **manage credentials**.

{{% pretty_screenshot img="/uploads/2020/10/git_manage_credentials.png" %}}

Tower offers support to connect to private repositories from popular git hosting platforms (GitHub, GitLab, and BitBucket)

{{% pretty_screenshot img="/uploads/2020/10/git_platforms.png" %}}

## Connect to a private GitHub repository

To connect a private GitHub repository you need to enter your **username** and **password** or **Access token**. GitHub recommends generating an access token instead of a password. Personal access tokens (PATs) are an alternative to using passwords for authentication to GitHub when using APIs.

Step-by-step instructions to create a personal access token can be found [here](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token).

## Connect to a private GitLab repository

To connect a private GitLab repository you need to enter your **username**, **password** and an **Access token**. The GitLab API access token that can be found in your [GitLab account page](https://gitlab.com/profile/personal_access_tokens). Make sure to select the `api`, `read_api`, and  `read_repository` options.

{{% pretty_screenshot img="/uploads/2020/10/git_gitlab_access_token.png" %}}

## Connect to a private Bitbucket repository

To connect a private BitBucket repository you need to enter your **username** and a **BitBucket App password**. [This step by step example](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/) shows how to create a BitBucket App password.

## Connect to a self-hosted git repository

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

These [Git access options](https://www.nextflow.io/docs/latest/sharing.html#scm-configuration-file Nextflow SCM configuration file) are Nextflow examples. You can first test your connection with a Nextflow execution using the standard SCM file and then convert it to the YAML structure, as shown above, for Tower.

{{% /tip %}}
