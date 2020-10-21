---
title: Public git repositories
menu:
  docs:
    parent: Git Integration
    weight: 2
---
As Nextflow data pipelines are simply Git repositories, these can be either public or private and hosted on any of the popular GitHub, GitLab or BitBucket platform.

Launching public hosted git pipelines simply requires adding the git repo URL in the **pipeline to launch** field. Note the revision numbers for the given repository are automatically loaded. By default, the stable `master` branch will be executed.

{{% pretty_screenshot img="/uploads/2020/10/git_public_repo.png" %}}

{{% tip %}}
[nf-core](https://nf-co.re/pipelines) is a great resource for public nextflow pipelines.
{{% /tip %}}
<br>
**[NEXT Connexting to a private Git repository →](/docs/git/git-private/)**
