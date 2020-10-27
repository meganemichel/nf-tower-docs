---
title: Scripting overview
aliases:
- "/docs/nextflow-scripting"
weight: 1
layout: single
publishdate: 2020-10-20 04:00:00 +0000
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
private: false
menu:
  docs:
    parent: Nextflow scripting
    weight: 1
---


The Nextflow scripting language is an extension of the Groovy programming language. Groovy is a powerful programming language for the Java virtual machine. The Nextflow syntax has been specialized to ease the writing of computational pipelines in a declarative manner.

Nextflow can execute any piece of Groovy code or use any library for the JVM platform.

For a detailed description of the Groovy programming language, reference these links:

-   [Groovy User Guide](http://groovy-lang.org/documentation.html)
-   [Groovy Cheat sheet](http://www.cheat-sheets.org/saved-copy/rc015-groovy_online.pdf)
-   [Groovy in Action](http://www.manning.com/koenig2/)

Below you can find a crash course in the most important language constructs used in the Nextflow scripting language.

{{% warning %}}

Nextflow uses `UTF-8` as the default file character encoding for source and application files. Make sure to use the `UTF-8` encoding when editing Nextflow scripts with your favourite text editor.

{{% /warning %}}
