---
title: Your first script
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
private: false
menu:
  docs:
    parent: Get started
    weight: 3
---

Copy the following example into your favourite text editor and save it
to a file named `tutorial.nf` :

{{< highlight bash >}}
    #!/usr/bin/env nextflow

    params.str = 'Hello world!'

    process splitLetters {

        output:
        file 'chunk_*' into letters

        """
        printf '${params.str}' | split -b 6 - chunk_
        """
    }


    process convertToUpper {

        input:
        file x from letters.flatten()

        output:
        stdout result

        """
        cat $x | tr '[a-z]' '[A-Z]'
        """
    }

    result.view { it.trim() }
{{< /highlight >}}

This script defines two processes. The first splits a string into
6-character chunks, writing each one to a file with the prefix `chunk_`,
and the second receives these files and transforms their contents to
uppercase letters. The resulting strings are emitted on the `result`
channel and the final output is printed by the `view` operator.

Execute the script by entering the following command in your terminal:

{{< highlight bash >}}
    nextflow run tutorial.nf
{{< /highlight >}}

It will output something similar to the text shown below:

{{< highlight bash >}}
    N E X T F L O W  ~  version 19.04.0
    executor >  local (3)
    [69/c8ea4a] process > splitLetters   [100%] 1 of 1 ✔
    [84/c8b7f1] process > convertToUpper [100%] 2 of 2 ✔
    HELLO
    WORLD!
{{< /highlight >}}

You can see that the first process is executed once, and the second
twice. Finally the result string is printed.

It's worth noting that the process `convertToUpper` is executed in
parallel, so there's no guarantee that the instance processing the first
split (the chunk _Hello_) will be executed
before the one processing the second split (the chunk <span
class="title-ref">world!</span>).

Thus, it is perfectly possible that you will get the final result
printed out in a different order:

{{< highlight bash >}}

    WORLD!
    HELLO
{{< /highlight >}}

{{% tip %}}

The hexadecimal numbers, like `22/7548fa`, identify the unique process
execution. These numbers are also the prefix of the directories where
each process is executed. You can inspect the files produced by them
changing to the directory `$PWD/work` and using these numbers to find
the process-specific execution path.

{{% /tip %}}

## Modify and resume

Nextflow keeps track of all the processes executed in your pipeline. If
you modify some parts of your script, only the processes that are
actually changed will be re-executed. The execution of the processes
that are not changed will be skipped and the cached result used instead.

This helps a lot when testing or modifying part of your pipeline without
having to re-execute it from scratch.

For the sake of this tutorial, modify the `convertToUpper` process in
the previous example, replacing the process script with the string
`rev $x`, so that the process looks like this:

{{< highlight groovy >}}
    process convertToUpper {

        input:
        file x from letters

        output:
        stdout result

        """
        rev $x
        """
    }
{{< /highlight >}}

Then save the file with the same name, and execute it by adding the
`-resume` option to the command line:

{{< highlight bash >}}
    nextflow run tutorial.nf -resume
{{< /highlight >}}

It will print output similar to this:

{{< highlight bash >}}
    N E X T F L O W  ~  version 19.04.0
    executor >  local (2)
    [69/c8ea4a] process > splitLetters   [100%] 1 of 1, cached: 1 ✔
    [d0/e94f07] process > convertToUpper [100%] 2 of 2 ✔
    olleH
    !dlrow
{{< /highlight >}}

You will see that the execution of the process `splitLetters` is
actually skipped (the process ID is the same), and its results are
retrieved from the cache. The second process is executed as expected,
printing the reversed strings.

{{% tip %}}

The pipeline results are cached by default in the directory `$PWD/work`.
Depending on your script, this folder can take of lot of disk space. If
you are sure you won't resume your pipeline execution, clean this folder
periodically.

{{% /tip %}}

## Pipeline parameters

Pipeline parameters are simply declared by prepending to a variable name
the prefix `params`, separated by dot character. Their value can be
specified on the command line by prefixing the parameter name with a
double <span class="title-ref">dash</span> character, i.e. `--paramName`

For the sake of this tutorial, you can try to execute the previous
example specifying a different input string parameter, as shown below:

{{< highlight bash >}}
    nextflow run tutorial.nf --str 'Bonjour le monde'
{{< /highlight >}}

The string specified on the command line will override the default value
of the parameter. The output will look like this:

{{< highlight bash >}}
    N E X T F L O W  ~  version 19.04.0
    executor >  local (4)
    [8b/16e7d7] process > splitLetters   [100%] 1 of 1 ✔
    [eb/729772] process > convertToUpper [100%] 3 of 3 ✔
    m el r
    edno
    uojnoB
{{< /highlight >}}
