---
title: Installation
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
private: false
menu:
  docs:
    parent: Get started
    weight: 2
---

_Nextflow_ is distributed as a
self-contained executable package, which means that it does not require
any special installation procedure.

It only needs two easy steps:

**1.**  Download the executable package by copying and pasting the following command in your terminal window:
`wget -qO- https://get.nextflow.io | bash`
It will create the
`nextflow` main executable file in the current directory.

**2.**  Optionally, move the `nextflow` file to a directory accessible by your `$PATH` variable (this is only required to avoid remembering and typing the full path to `nextflow` each time you need to run it).

{{% tip %}}

In the case you don't have `wget` installed you can use the `curl`
utility instead by entering the following command:
`curl -s https://get.nextflow.io | bash`

{{% /tip %}}
