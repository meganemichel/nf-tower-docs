---
title: Processes and channels
layout: single
publishdate: 2020-06-11 04:00:00 +0000
expirydate: 2030-01-01 04:00:00 +0000
date: 2020-06-11 04:00:00 +0000
private: false
headline: 'Basic concepts'
menu:
  docs:
    parent: Basic concepts
    weight: 1
---

_Nextflow_ is a reactive workflow framework
and a programming
[DSL](http://en.wikipedia.org/wiki/Domain-specific_language) that eases
the writing of data-intensive computational pipelines.

It is designed around the idea that the Linux platform is the lingua
franca of data science. Linux provides many simple but powerful
command-line and scripting tools that, when chained together, facilitate
complex data manipulations.

_Nextflow_ extends this approach, adding
the ability to define complex program interactions and a high-level
parallel computational environment based on the _dataflow_ programming model.

# Processes and channels

In practice a Nextflow pipeline script is made by joining together
different processes. Each process can be written in any scripting
language that can be executed by the Linux platform (Bash, Perl, Ruby,
Python, etc.).

Processes are executed independently and are isolated from each other,
i.e. they do not share a common (writable) state. The only way they can
communicate is via asynchronous FIFO queues, called _channels_ in Nextflow.

Any process can define one or more channels as
_input_
and _output_. The interaction between these
processes, and ultimately the pipeline execution flow itself, is
implicitly defined by these input and output declarations.

A Nextflow script looks like this:

{{< highlight groovy >}}
    // Script parameters
    params.query = "/some/data/sample.fa"
    params.db = "/some/path/pdb"

    db = file(params.db)
    query_ch = Channel.fromPath(params.query)

    process blastSearch {
        input:
        file query from query_ch

        output:
        file "top_hits.txt" into top_hits_ch

        """
        blastp -db $db -query $query -outfmt 6 > blast_result
        cat blast_result | head -n 10 | cut -f 2 > top_hits.txt
        """
    }

    process extractTopHits {
        input:
        file top_hits from top_hits_ch

        output:
        file "sequences.txt" into sequences_ch

        """
        blastdbcmd -db $db -entry_batch $top_hits > sequences.txt
        """
    }
{{< /highlight >}}

The above example defines two processes. Their execution order is not determined by the fact that the `blastSearch` process comes before `extractTopHits` in the script (it could also be written the other way around).

Instead, because the first process defines the channel `top_hits_ch` in its output declarations, and the process `extractTopHits` defines the channel in its input declaration, a communication link is established.

This linking via the channels means that _extractTopHits_ is waiting for the output of _blastSearch_, and then runs _reactively_ when the channel has contents.

Read the `Channel <channel-page>` and `Process <process-page>` sections to learn more about these features.
