---
title: Private git repositories
aliases:
- "/docs/git-private"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
authors:
  - "Evan Floden"
  - "Alain Coletta"
  - "Seqera Labs"
headline: "Executing workflows hosted in a private git repository"
description: 'Managing and connecting to workflow git repositories'


menu:
  docs:
    parent: Git Integration
    weight: 3
---

{{% warning %}}
All credentials are securely stored using advanced encryption (AES-256) and never exposed by any Tower API.
{{% /warning %}}

Credentials for private git repositories can be managed from the [credentials page](https://tower.nf/credentials) accessible on the right top menu, under **manage credentials**.

{{% pretty_screenshot img="/uploads/2020/10/git_manage_credentials.png" %}}

Tower offers support to connect to private repositories from popular git hosting platforms (GitHub, GitLab, and BitBucket)

{{% pretty_screenshot img="/uploads/2020/10/git_platforms.png" %}}

## GitHub

To connect a private GitHub repository you need to enter your **username** and **password** or **Access token**. GitHub recommends generating an access token instead of a password. Personal access tokens (PATs) are an alternative to using passwords for authentication to GitHub when using APIs.

Step-by-step instructions to create a personal access token can be found [here](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token).

## GitLab

To connect a private GitLab repository you need to enter your **username**, **password** and an **Access token**. The GitLab API access token that can be found in your [GitLab account page](https://gitlab.com/profile/personal_access_tokens). Make sure to select the `api`, `read_api`, and  `read_repository` options.

{{% pretty_screenshot img="/uploads/2020/10/git_gitlab_access_token.png" %}}

## Bitbucket

To connect a private BitBucket repository you need to enter your **username** and a **BitBucket App password**. [This step by step example](https://support.atlassian.com/bitbucket-cloud/docs/app-passwords/) shows how to create a BitBucket App password.

## Self-hosted git repositories

It is also possible to specify Git server endpoints for private hosting.

These can be specified in a file `tower.yaml` and must be accessible from the backend and cron container instances.

{{< highlight yaml >}}
tower:
  scm:
    providers:
      my_org_bitbucket:
        server: "https://bitbucket.my-org.com"
        endpoint: "<the API endpoint URL if different from the above>"
        platform: bitbucketserver
        user: some_user_name
        password: password_or_access_token
{{< /highlight >}}

{{% tip %}}

These [Git access options](https://www.nextflow.io/docs/latest/sharing.html#scm-configuration-file Nextflow SCM configuration file) are Nextflow examples. You can first test your connection with a Nextflow execution using the standard SCM file and then convert it to the YAML structure, as shown above, for Tower.

{{% /tip %}}
