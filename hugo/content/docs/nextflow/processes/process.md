---
title: Script
aliases:
- "/docs/nextflow-scripting"
headline: 'Processes'
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
    parent: Processes
    weight: 1
---


In Nextflow a <span class="title-ref">process</span> is the basic
processing <span class="title-ref">primitive</span> to execute a user
script.

The process definition starts with keyword the `process`, followed by
process name and finally the process <span class="title-ref">body</span>
delimited by curly brackets. The process body must contain a string
which represents the command or, more generally, a script that is
executed by it. A basic process looks like the following example:

    process sayHello {

        """
        echo 'Hello world!' > file
        """

    }

A process may contain five definition blocks, respectively: directives,
inputs, outputs, when clause and finally the process script. The syntax
is defined as follows:

    process < name > {

       [ directives ]

       input:
        < process inputs >

       output:
        < process outputs >

       when:
        < condition >

       [script|shell|exec]:
       < user script to be executed >

    }


# Script


The <span class="title-ref">script</span> block is a string statement
that defines the command that is executed by the process to carry out
its task.

A process contains one and only one script block, and it must be the
last statement when the process contains input and output declarations.

The entered string is executed as a
[Bash](http://en.wikipedia.org/wiki/Bash_(Unix_shell)) script in the
<span class="title-ref">host</span> system. It can be any command,
script or combination of them, that you would normally use in terminal
shell or in a common Bash script.

The only limitation to the commands that can be used in the script
statement is given by the availability of those programs in the target
execution system.

The script block can be a simple string or multi-line string. The latter
simplifies the writing of non trivial scripts composed by multiple
commands spanning over multiple lines. For example:

    process doMoreThings {

      """
      blastp -db $db -query query.fa -outfmt 6 > blast_result
      cat blast_result | head -n 10 | cut -f 2 > top_hits
      blastdbcmd -db $db -entry_batch top_hits > sequences
      """

    }

As explained in the script tutorial section, strings can be defined by
using a single-quote or a double-quote, and multi-line strings are
defined by three single-quote or three double-quote characters.

There is a subtle but important difference between them. Like in Bash,
strings delimited by a `"` character support variable substitutions,
while strings delimited by `'` do not.

In the above code fragment the `$db` variable is replaced by the actual
value defined somewhere in the pipeline script.

{{% warning %}}

Since Nextflow uses the same Bash syntax for variable substitutions in
strings, you need to manage them carefully depending on if you want to
evaluate a variable in the Nextflow context - or - in the Bash
environment execution.

{{% /warning %}}

When you need to access a system environment variable in your script you
have two options. The first choice is as easy as defining your script
block by using a single-quote string. For example:

    process printPath {

       '''
       echo The path is: $PATH
       '''

    }

The drawback of this solution is that you will not able to access
variables defined in the pipeline script context, in your script block.

To fix this, define your script by using a double-quote string and <span
class="title-ref">escape</span> the system environment variables by
prefixing them with a back-slash `\` character, as shown in the
following example:

    process doOtherThings {

      """
      blastp -db \$DB -query query.fa -outfmt 6 > blast_result
      cat blast_result | head -n $MAX | cut -f 2 > top_hits
      blastdbcmd -db \$DB -entry_batch top_hits > sequences
      """

    }

In this example the `$MAX` variable has to be defined somewhere before,
in the pipeline script. <span class="title-ref">Nextflow</span> replaces
it with the actual value before executing the script. Instead, the `$DB`
variable must exist in the script execution environment and the Bash
interpreter will replace it with the actual value.

{{% tip %}}

Alternatively you can use the `process-shell` block definition which
allows a script to contain both Bash and Nextflow variables without
having to escape the first.

{{% /tip %}}

### Scripts <span class="title-ref">à la carte</span>

The process script is interpreted by Nextflow as a Bash script by
default, but you are not limited to it.

You can use your favourite scripting language (e.g. Perl, Python, Ruby,
R, etc), or even mix them in the same pipeline.

A pipeline may be composed by processes that execute very different
tasks. Using <span class="title-ref">Nextflow</span> you can choose the
scripting language that better fits the task carried out by a specified
process. For example for some processes <span class="title-ref">R</span>
could be more useful than <span class="title-ref">Perl</span>, in other
you may need to use <span class="title-ref">Python</span> because it
provides better access to a library or an API, etc.

To use a scripting other than Bash, simply start your process script
with the corresponding
[shebang](http://en.wikipedia.org/wiki/Shebang_(Unix)) declaration. For
example:

    process perlStuff {

        """
        #!/usr/bin/perl

        print 'Hi there!' . '\n';
        """

    }

    process pyStuff {

        """
        #!/usr/bin/python

        x = 'Hello'
        y = 'world!'
        print "%s - %s" % (x,y)
        """

    }

{{% tip %}}

Since the actual location of the interpreter binary file can change
across platforms, to make your scripts more portable it is wise to use
the `env` shell command followed by the interpreter's name, instead of
the absolute path of it. Thus, the <span
class="title-ref">shebang</span> declaration for a Perl script, for
example, would look like: `#!/usr/bin/env perl` instead of the one in
the above pipeline fragment.

</div>

### Conditional scripts

Complex process scripts may need to evaluate conditions on the input
parameters or use traditional flow control statements (i.e. `if`,
`switch`, etc) in order to execute specific script commands, depending
on the current inputs configuration.

Process scripts can contain conditional statements by simply prefixing
the script block with the keyword `script:`. By doing that the
interpreter will evaluate all the following statements as a code block
that must return the script string to be executed. It's much easier to
use than to explain, for example:

    seq_to_align = ...
    mode = 'tcoffee'

    process align {
        input:
        file seq_to_aln from sequences

        script:
        if( mode == 'tcoffee' )
            """
            t_coffee -in $seq_to_aln > out_file
            """

        else if( mode == 'mafft' )
            """
            mafft --anysymbol --parttree --quiet $seq_to_aln > out_file
            """

        else if( mode == 'clustalo' )
            """
            clustalo -i $seq_to_aln -o out_file
            """

        else
            error "Invalid alignment mode: ${mode}"

    }

In the above example the process will execute the script fragment
depending on the value of the `mode` parameter. By default it will
execute the `tcoffee` command, changing the `mode` variable to `mafft`
or `clustalo` value, the other branches will be executed.

### Template

Process script can be externalised by using *template* files which can
be reused across different processes and tested independently by the
overall pipeline execution.

A template is simply a shell script file that Nextflow is able to
execute by using the `template` function as shown below:

    process template_example {

        input:
        val STR from 'this', 'that'

        script:
        template 'my_script.sh'

    }

Nextflow looks for the `my_script.sh` template file in the directory
`templates` that must exist in the same folder where the Nextflow script
file is located (any other location can be provided by using an absolute
template path).

The template script can contain any piece of code that can be executed
by the underlying system. For example:

    #!/bin/bash
    echo "process started at `date`"
    echo $STR
    :
    echo "process completed"

{{% tip %}}

Note that the dollar character (`$`) is interpreted as a Nextflow
variable placeholder, when the script is run as a Nextflow template,
while it is evaluated as a Bash variable when it is run alone. This can
be very useful to test your script autonomously, i.e. independently from
Nextflow execution. You only need to provide a Bash environment variable
for each the Nextflow variable existing in your script. For example, it
would be possible to execute the above script entering the following
command in the shell terminal: `STR='foo' bash templates/my_script.sh`

</div>

### Shell

The `shell` block is a string statement that defines the *shell* command
executed by the process to carry out its task. It is an alternative to
the `process-script` definition with an important difference, it uses
the exclamation mark `!` character as the variable placeholder for
Nextflow variables in place of the usual dollar character.

In this way it is possible to use both Nextflow and Bash variables in
the same piece of code without having to escape the latter and making
process scripts more readable and easy to maintain. For example:

    process myTask {

        input:
        val str from 'Hello', 'Hola', 'Bonjour'

        shell:
        '''
        echo User $USER says !{str}
        '''

    }

In the above trivial example the `$USER` variable is managed by the Bash
interpreter, while `!{str}` is handled as a process input variable
managed by Nextflow.

{{% note %}}

-   Shell script definition requires the use of single-quote `'`
    delimited strings. When using double-quote `"` delimited strings,
    dollar variables are interpreted as Nextflow variables as usual. See
    `string-interpolation`.
-   Exclamation mark prefixed variables always need to be enclosed in
    curly brackets i.e. `!{str}` is a valid variable while `!str` is
    ignored.
-   Shell script supports the use of the file `process-template`
    mechanism. The same rules are applied to the variables defined in
    the script template.

</div>

### Native execution

Nextflow processes can execute native code other than system scripts as
shown in the previous paragraphs.

This means that instead of specifying the process command to be executed
as a string script, you can define it by providing one or more language
statements, as you would do in the rest of the pipeline script. Simply
starting the script definition block with the `exec:` keyword, for
example:

    x = Channel.from( 'a', 'b', 'c')

    process simpleSum {
        input:
        val x

        exec:
        println "Hello Mr. $x"
    }

Will display:

    Hello Mr. b
    Hello Mr. a
    Hello Mr. c

Inputs
------

Nextflow processes are isolated from each other but can communicate
between themselves sending values through channels.

The <span class="title-ref">input</span> block defines which channels
the process is expecting to receive inputs data from. You can only
define one input block at a time and it must contain one or more inputs
declarations.

The input block follows the syntax shown below:

    input:
      <input qualifier> <input name> [from <source channel>] [attributes]

An input definition starts with an input <span
class="title-ref">qualifier</span> and the input <span
class="title-ref">name</span>, followed by the keyword `from` and the
actual channel over which inputs are received. Finally some input
optional attributes can be specified.

{{% note %}}

When the input name is the same as the channel name, the `from` part of
the declaration can be omitted.

</div>

The input qualifier declares the <span class="title-ref">type</span> of
data to be received. This information is used by Nextflow to apply the
semantic rules associated to each qualifier and handle it properly
depending on the target execution platform (grid, cloud, etc).

The qualifiers available are the ones listed in the following table:

| Qualifier | Semantic                                                                                              |
|-----------|-------------------------------------------------------------------------------------------------------|
| val       | Lets you access the received input value by its name in the process script.                           |
| env       | Lets you use the received value to set an environment variable named as the specified input name.     |
| file      | Lets you handle the received value as a file, staging it properly in the execution context.           |
| path      | Lets you handle the received value as a path, staging the file properly in the execution context.     |
| stdin     | Lets you forward the received value to the process <span class="title-ref">stdin</span> special file. |
| tuple     | Lets you handle a group of input values having one of the above qualifiers.                           |
| each      | Lets you execute the process for each entry in the input collection.                                  |

### Input of generic values

The `val` qualifier allows you to receive data of any type as input. It
can be accessed in the process script by using the specified input name,
as shown in the following example:

    num = Channel.from( 1, 2, 3 )

    process basicExample {
      input:
      val x from num

      "echo process job $x"

    }

In the above example the process is executed three times, each time a
value is received from the channel `num` and used to process the script.
Thus, it results in an output similar to the one shown below:

    process job 3
    process job 1
    process job 2

{{% note %}}

The <span class="title-ref">channel</span> guarantees that items are
delivered in the same order as they have been sent - but -since the
process is executed in a parallel manner, there is no guarantee that
they are processed in the same order as they are received. In fact, in
the above example, value `3` is processed before the others.

</div>

When the `val` has the same name as the channel from where the data is
received, the `from` part can be omitted. Thus the above example can be
written as shown below:

    num = Channel.from( 1, 2, 3 )

    process basicExample {
      input:
      val num

      "echo process job $num"

    }

### Input of files

The `file` qualifier allows the handling of file values in the process
execution context. This means that Nextflow will stage it in the process
execution directory, and it can be access in the script by using the
name specified in the input declaration. For example:

    proteins = Channel.fromPath( '/some/path/*.fa' )

    process blastThemAll {
      input:
      file query_file from proteins

      "blastp -query ${query_file} -db nr"

    }

In the above example all the files ending with the suffix `.fa` are sent
over the channel `proteins`. Then, these files are received by the
process which will execute a <span class="title-ref">BLAST</span> query
on each of them.

When the file input name is the same as the channel name, the `from`
part of the input declaration can be omitted. Thus, the above example
could be written as shown below:

    proteins = Channel.fromPath( '/some/path/*.fa' )

    process blastThemAll {
      input:
      file proteins

      "blastp -query $proteins -db nr"

    }

It's worth noting that in the above examples, the name of the file in
the file-system is not touched, you can access the file even without
knowing its name because you can reference it in the process script by
using the variable whose name is specified in the input file parameter
declaration.

There may be cases where your task needs to use a file whose name is
fixed, it does not have to change along with the actual provided file.
In this case you can specify its name by specifying the `name` attribute
in the input file parameter declaration, as shown in the following
example:

    input:
        file query_file name 'query.fa' from proteins

Or alternatively using a shorter syntax:

    input:
        file 'query.fa' from proteins

Using this, the previous example can be re-written as shown below:

    proteins = Channel.fromPath( '/some/path/*.fa' )

    process blastThemAll {
      input:
      file 'query.fa' from proteins

      "blastp -query query.fa -db nr"

    }

What happens in this example is that each file, that the process
receives, is staged with the name `query.fa` in a different execution
context (i.e. the folder where the job is executed) and an independent
process execution is launched.

{{% tip %}}

This allows you to execute the process command various time without
worrying the files names changing. In other words, <span
class="title-ref">Nextflow</span> helps you write pipeline tasks that
are self-contained and decoupled by the execution environment. This is
also the reason why you should avoid whenever possible to use absolute
or relative paths referencing files in your pipeline processes.

</div>

### Multiple input files

A process can declare as input file a channel that emits a collection of
values, instead of a simple value.

In this case, the script variable defined by the input file parameter
will hold a list of files. You can use it as shown before, referring to
all the files in the list, or by accessing a specific entry using the
usual square brackets notation.

When a target file name is defined in the input parameter and a
collection of files is received by the process, the file name will be
appended by a numerical suffix representing its ordinal position in the
list. For example:

    fasta = Channel.fromPath( "/some/path/*.fa" ).buffer(size:3)

    process blastThemAll {
        input:
        file 'seq' from fasta

        "echo seq*"

    }

Will output:

    seq1 seq2 seq3
    seq1 seq2 seq3
    ...

The target input file name can contain the `*` and `?` wildcards, that
can be used to control the name of staged files. The following table
shows how the wildcards are replaced depending on the cardinality of the
received input collection.

<table>
<thead>
<tr class="header">
<th>Cardinality</th>
<th>Name pattern</th>
<th>Staged file names</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p>any</p>
</blockquote></td>
<td><code>*</code></td>
<td><blockquote>
<p>named as the source file</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><code>file*.ext</code></td>
<td><blockquote>
<p><code>file.ext</code></p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><code>file?.ext</code></td>
<td><blockquote>
<p><code>file1.ext</code></p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>1</p>
</blockquote></td>
<td><code>file??.ext</code></td>
<td><blockquote>
<p><code>file01.ext</code></p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>many</p>
</blockquote></td>
<td><code>file*.ext</code></td>
<td><blockquote>
<p><code>file1.ext</code>, <code>file2.ext</code>, <code>file3.ext</code>, ..</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>many</p>
</blockquote></td>
<td><code>file?.ext</code></td>
<td><blockquote>
<p><code>file1.ext</code>, <code>file2.ext</code>, <code>file3.ext</code>, ..</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>many</p>
</blockquote></td>
<td><code>file??.ext</code></td>
<td><blockquote>
<p><code>file01.ext</code>, <code>file02.ext</code>, <code>file03.ext</code>, ..</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>many</p>
</blockquote></td>
<td><code>dir/*</code></td>
<td><blockquote>
<p>named as the source file, created in <code>dir</code> subdirectory</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><blockquote>
<p>many</p>
</blockquote></td>
<td><code>dir??/*</code></td>
<td><blockquote>
<p>named as the source file, created in a progressively indexed subdirectory e.g. <code>dir01/</code>, <code>dir02/</code>, etc.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><blockquote>
<p>many</p>
</blockquote></td>
<td><code>dir*/*</code></td>
<td><blockquote>
<p>(as above)</p>
</blockquote></td>
</tr>
</tbody>
</table>

The following fragment shows how a wildcard can be used in the input
file declaration:

    fasta = Channel.fromPath( "/some/path/*.fa" ).buffer(size:3)

    process blastThemAll {
        input:
        file 'seq?.fa' from fasta

        "cat seq1.fa seq2.fa seq3.fa"

    }

{{% note %}}

Rewriting input file names according to a named pattern is an extra
feature and not at all obligatory. The normal file input constructs
introduced in the [Input of files](#input-of-files) section are valid
for collections of multiple files as well. To handle multiple input
files preserving the original file names, use the `*` wildcard as name
pattern or a variable identifier.

</div>

### Dynamic input file names

When the input file name is specified by using the `name` file clause or
the short <span class="title-ref">string</span> notation, you are
allowed to use other input values as variables in the file name string.
For example:

    process simpleCount {
      input:
      val x from species
      file "${x}.fa" from genomes

      """
      cat ${x}.fa | grep '>'
      """
    }

In the above example, the input file name is set by using the current
value of the `x` input value.

This allows the input files to be staged in the script working directory
with a name that is coherent with the current execution context.

{{% tip %}}

In most cases, you won't need to use dynamic file names, because each
process is executed in its own private temporary directory, and input
files are automatically staged to this directory by Nextflow. This
guarantees that input files with the same name won't overwrite each
other.

</div>

### Input of type 'path'

The `path` input qualifier was introduced by Nextflow version 19.10.0
and it's a drop-in replacement for the `file` qualifier, therefore it's
backward compatible with the syntax and the semantic for the input
`file` described above.

The important difference between `file` and `path` qualifier is that the
first expects the values received as input to be *file* objects. When
inputs is a different type, it automatically coverts to a string and
saves it to a temporary files. This can be useful in some uses cases,
but it turned out to be tricky in most common cases.

The `path` qualifier instead interprets string values as the path
location of the input file and automatically converts to a file object.

    process foo {
      input:
        path x from '/some/data/file.txt'
      """
        your_command --in $x
      """
    }

{{% note %}}

Provided input value should represent an absolute path location i.e. the
string value **must** be prefixed with a <span
class="title-ref">/</span> character or with a supported URI protocol
i.e. `file://`, `http://`, `s3://`, etc. and it cannot contains special
characters (e.g. `\n`, etc.).

</div>

The option `stageAs` allow you to control how the file should be named
in the task work directory, providing a specific name or a name pattern
as described in the [Multiple input files](#multiple-input-files)
section:

    process foo {
      input:
        path x, stageAs: 'data.txt' from '/some/data/file.txt'
      """
        your_command --in data.txt
      """
    }

{{% tip %}}

The `path` qualifier should be preferred over `file` to handle process
input files when using Nextflow 19.10.0 or later.

</div>

### Input of type 'stdin'

The `stdin` input qualifier allows you the forwarding of the value
received from a channel to the [standard
input](http://en.wikipedia.org/wiki/Standard_streams#Standard_input_.28stdin.29)
of the command executed by the process. For example:

    str = Channel.from('hello', 'hola', 'bonjour', 'ciao').map { it+'\n' }

    process printAll {
       input:
       stdin str

       """
       cat -
       """

    }

It will output:

    hola
    bonjour
    ciao
    hello

### Input of type 'env'

The `env` qualifier allows you to define an environment variable in the
process execution context based on the value received from the channel.
For example:

    str = Channel.from('hello', 'hola', 'bonjour', 'ciao')

    process printEnv {

        input:
        env HELLO from str

        '''
        echo $HELLO world!
        '''

    }

    hello world!
    ciao world!
    bonjour world!
    hola world!

### Input of type 'set'

{{% warning %}}

The <span class="title-ref">set</span> input type has been deprecated.
See <span class="title-ref">tuple</span> instead.

{{% /warning %}}

### Input of type 'tuple'

The `tuple` qualifier allows you to group multiple parameters in a
single parameter definition. It can be useful when a process receives,
in input, tuples of values that need to be handled separately. Each
element in the tuple is associated to a corresponding element with the
`tuple` definition. For example:

    values = Channel.of( [1, 'alpha'], [2, 'beta'], [3, 'delta'] )

    process tupleExample {
        input:
        tuple val(x), file('latin.txt') from values

        """
        echo Processing $x
        cat - latin.txt > copy
        """

    }

In the above example the `tuple` parameter is used to define the value
`x` and the file `latin.txt`, which will receive a value from the same
channel.

In the `tuple` declaration items can be defined by using the following
qualifiers: `val`, `env`, `file` and `stdin`.

A shorter notation can be used by applying the following substitution
rules:

<table>
<thead>
<tr class="header">
<th>long</th>
<th>short</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>val(x)</td>
<td><blockquote>
<p>x</p>
</blockquote></td>
</tr>
<tr class="even">
<td>file(x)</td>
<td><blockquote>
<p>(not supported)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>file('name')</td>
<td><blockquote>
<p>'name'</p>
</blockquote></td>
</tr>
<tr class="even">
<td>file(x:'name')</td>
<td><blockquote>
<p>x:'name'</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>stdin</td>
<td><blockquote>
<p>'-'</p>
</blockquote></td>
</tr>
<tr class="even">
<td>env(x)</td>
<td><blockquote>
<p>(not supported)</p>
</blockquote></td>
</tr>
</tbody>
</table>

Thus the previous example could be rewritten as follows:

    values = Channel.of( [1, 'alpha'], [2, 'beta'], [3, 'delta'] )

    process tupleExample {
        input:
        tuple x, 'latin.txt' from values

        """
        echo Processing $x
        cat - latin.txt > copy
        """
    }

File names can be defined in *dynamic* manner as explained in the
[Dynamic input file names](#dynamic-input-file-names) section.

### Input repeaters

The `each` qualifier allows you to repeat the execution of a process for
each item in a collection, every time a new data is received. For
example:

    sequences = Channel.fromPath('*.fa')
    methods = ['regular', 'expresso', 'psicoffee']

    process alignSequences {
      input:
      file seq from sequences
      each mode from methods

      """
      t_coffee -in $seq -mode $mode > result
      """
    }

In the above example every time a file of sequences is received as input
by the process, it executes *three* tasks running a T-coffee alignment
with a different value for the `mode` parameter. This is useful when you
need to <span class="title-ref">repeat</span> the same task for a given
set of parameters.

Since version 0.25+ input repeaters can be applied to files as well. For
example:

    sequences = Channel.fromPath('*.fa')
    methods = ['regular', 'expresso']
    libraries = [ file('PQ001.lib'), file('PQ002.lib'), file('PQ003.lib') ]

    process alignSequences {
      input:
      file seq from sequences
      each mode from methods
      each file(lib) from libraries

      """
      t_coffee -in $seq -mode $mode -lib $lib > result
      """
    }

{{% note %}}

When multiple repeaters are declared, the process is executed for each
*combination* of them.

</div>

In the latter example for any sequence input file emitted by the
`sequences` channel are executed 6 alignments, 3 using the `regular`
method against each library files, and other 3 by using the `expresso`
method always against the same library files.

{{% tip %}}

If you need to repeat the execution of a process over n-tuple of
elements instead a simple values or files, create a channel combining
the input values as needed to trigger the process execution multiple
times. In this regard, see the `operator-combine`, `operator-cross` and
`operator-phase` operators.

</div>

### Understand how multiple input channels work

A key feature of processes is the ability to handle inputs from multiple
channels.

When two or more channels are declared as process inputs, the process
stops until there's a complete input configuration ie. it receives an
input value from all the channels declared as input.

When this condition is verified, it consumes the input values coming
from the respective channels, and spawns a task execution, then repeat
the same logic until one or more channels have no more content.

This means channel values are consumed serially one after another and
the first empty channel cause the process execution to stop even if
there are other values in other channels.

For example:

    process foo {
      echo true
      input:
      val x from Channel.from(1,2)
      val y from Channel.from('a','b','c')
      script:
       """
       echo $x and $y
       """
    }

The process `foo` is executed two times because the first input channel
only provides two values and therefore the `c` element is discarded. It
prints:

    1 and a
    2 and b

{{% warning %}}

A different semantic is a applied when using *Value channel* a.k.a.
*Singleton channel*.

{{% /warning %}}

This kind of channel is created by the `Channel.value <channel-value>`
factory method or implicitly when a process input specifies a simple
value in the `from` clause.

By definition, a *Value channel* is bound to a single value and it can
be read unlimited times without consuming its content.

These properties make that when mixing a *value channel* with one or
more (queue) channels, it does not affect the process termination which
only depends by the other channels and its content is applied
repeatedly.

To better understand this behavior compare the previous example with the
following one:

    process bar {
      echo true
      input:
      val x from Channel.value(1)
      val y from Channel.from('a','b','c')
      script:
       """
       echo $x and $y
       """
    }

The above snippet executes the `bar` process three times because the
first input is a *value channel*, therefore its content can be read as
many times as needed. The process termination is determined by the
content of the second channel. It prints:

    1 and a
    1 and b
    1 and c

See also: `channel-types`.

Outputs
-------

The <span class="title-ref">output</span> declaration block allows to
define the channels used by the process to send out the results
produced.

It can be defined at most one output block and it can contain one or
more outputs declarations. The output block follows the syntax shown
below:

    output:
      <output qualifier> <output name> [into <target channel>[,channel,..]] [attribute [,..]]

Output definitions start by an output <span
class="title-ref">qualifier</span> and the output <span
class="title-ref">name</span>, followed by the keyword `into` and one or
more channels over which outputs are sent. Finally some optional
attributes can be specified.

{{% note %}}

When the output name is the same as the channel name, the `into` part of
the declaration can be omitted.

</div>

The qualifiers that can be used in the output declaration block are the
ones listed in the following table:

| Qualifier | Semantic                                                                                                |
|-----------|---------------------------------------------------------------------------------------------------------|
| val       | Sends variable's with the name specified over the output channel.                                       |
| file      | Sends a file produced by the process with the name specified over the output channel.                   |
| path      | Sends a file produced by the process with the name specified over the output channel (replaces `file`). |
| env       | Sends the variable defined in the process environment with the name specified over the output channel.  |
| stdout    | Sends the executed process <span class="title-ref">stdout</span> over the output channel.               |
| tuple     | Lets to send multiple values over the same output channel.                                              |

### Output values

The `val` qualifier allows to output a <span
class="title-ref">value</span> defined in the script context. In a
common usage scenario, this is a value which has been defined in the
<span class="title-ref">input</span> declaration block, as shown in the
following example:

    methods = ['prot','dna', 'rna']

    process foo {
      input:
      val x from methods

      output:
      val x into receiver

      """
      echo $x > file
      """

    }

    receiver.view { "Received: $it" }

Valid output values are value literals, input values identifiers,
variables accessible in the process scope and value expressions. For
example:

    process foo {
      input:
      file fasta from 'dummy'

      output:
      val x into var_channel
      val 'BB11' into str_channel
      val "${fasta.baseName}.out" into exp_channel

      script:
      x = fasta.name
      """
      cat $x > file
      """
    }

### Output files

The `file` qualifier allows to output one or more files, produced by the
process, over the specified channel. For example:

    process randomNum {

       output:
       file 'result.txt' into numbers

       '''
       echo $RANDOM > result.txt
       '''

    }

    numbers.subscribe { println "Received: " + it.text }

In the above example the process, when executed, creates a file named
`result.txt` containing a random number. Since a file parameter using
the same name is declared between the outputs, when the task is
completed that file is sent over the `numbers` channel. A downstream
<span class="title-ref">process</span> declaring the same channel as
<span class="title-ref">input</span> will be able to receive it.

{{% note %}}

If the channel specified as output has not been previously declared in
the pipeline script, it will implicitly created by the output
declaration itself.

</div>

### Multiple output files

When an output file name contains a `*` or `?` wildcard character it is
interpreted as a
[glob](http://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob)
path matcher. This allows to *capture* multiple files into a list object
and output them as a sole emission. For example:

    process splitLetters {

        output:
        file 'chunk_*' into letters

        '''
        printf 'Hola' | split -b 1 - chunk_
        '''
    }

    letters
        .flatMap()
        .subscribe { println "File: ${it.name} => ${it.text}" }

It prints:

    File: chunk_aa => H
    File: chunk_ab => o
    File: chunk_ac => l
    File: chunk_ad => a

{{% note %}}

In the above example the operator `operator-flatmap` is used to
transform the list of files emitted by the `letters` channel into a
channel that emits each file object independently.

</div>

Some caveats on glob pattern behavior:

-   Input files are not included in the list of possible matches.
-   Glob pattern matches against both files and directories path.
-   When a two stars pattern `**` is used to recourse across
    directories, only file paths are matched i.e. directories are not
    included in the result list.

{{% warning %}}

Although the input files matching a glob output declaration are not
included in the resulting output channel, these files may still be
transferred from the task scratch directory to the target task work
directory. Therefore, to avoid unnecessary file copies it is recommended
to avoid the usage of loose wildcards when defining output files e.g.
`file '*'` . Instead, use a prefix or a postfix naming notation to
restrict the set of matching files to only the expected ones e.g.
`file 'prefix_*.sorted.bam'`.

{{% /warning %}}

By default all the files matching the specified glob pattern are emitted
by the channel as a sole (list) item. It is also possible to emit each
file as a sole item by adding the `mode flatten` attribute in the output
file declaration.

By using the `mode` attribute the previous example can be re-written as
show below:

    process splitLetters {

        output:
        file 'chunk_*' into letters mode flatten

        '''
        printf 'Hola' | split -b 1 - chunk_
        '''
    }

    letters .subscribe { println "File: ${it.name} => ${it.text}" }

{{% warning %}}

The option `mode` is deprecated as of version 19.10.0. Use the operator
`operator-collect` in the downstream process instead.

{{% /warning %}}

Read more about glob syntax at the following link [What is a
glob?](http://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob)

### Dynamic output file names

When an output file name needs to be expressed dynamically, it is
possible to define it using a dynamic evaluated string which references
values defined in the input declaration block or in the script global
context. For example:

    process align {
      input:
      val x from species
      file seq from sequences

      output:
      file "${x}.aln" into genomes

      """
      t_coffee -in $seq > ${x}.aln
      """
    }

In the above example, each time the process is executed an alignment
file is produced whose name depends on the actual value of the `x`
input.

{{% tip %}}

The management of output files is a very common misunderstanding when
using Nextflow. With other tools, it is generally necessary to organize
the outputs files into some kind of directory structure or to guarantee
a unique file name scheme, so that result files won't overwrite each
other and that they can be referenced univocally by downstream tasks.

With Nextflow, in most cases, you don't need to take care of naming
output files, because each task is executed in its own unique temporary
directory, so files produced by different tasks can never override each
other. Also meta-data can be associated with outputs by using the
`tuple output <process-out-tuple>` qualifier, instead of including them
in the output file name.

To sum up, the use of output files with static names over dynamic ones
is preferable whenever possible, because it will result in a simpler and
more portable code.

</div>

### Output path

The `path` output qualifier was introduced by Nextflow version 19.10.0
and it's a drop-in replacement for the `file` output qualifier,
therefore it's backward compatible with the syntax and the semantic for
the input `file` described above.

The main advantage of `path` over the `file` qualifier is that it allows
the specification of a number of output to fine-control the output
files.

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>glob</td>
<td><blockquote>
<p>When <code>true</code> the specified name is interpreted as a glob pattern (default: <code>true</code>)</p>
</blockquote></td>
</tr>
<tr class="even">
<td>hidden</td>
<td><blockquote>
<p>When <code>true</code> hidden files are included in the matching output files (default: <code>false</code>)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>followLinks</td>
<td><blockquote>
<p>When <code>true</code> target files are return in place of any matching symlink (default: <code>true</code>)</p>
</blockquote></td>
</tr>
<tr class="even">
<td>type</td>
<td><blockquote>
<p>Type of paths returned, either <code>file</code>, <code>dir</code> or <code>any</code> (default: <code>any</code>, or <code>file</code> if the specified file name pattern contains a <span class="title-ref">**</span> - double star - symbol)</p>
</blockquote></td>
</tr>
<tr class="odd">
<td>maxDepth</td>
<td><blockquote>
<p>Maximum number of directory levels to visit (default: <span class="title-ref">no limit</span>)</p>
</blockquote></td>
</tr>
<tr class="even">
<td>includeInputs</td>
<td><blockquote>
<p>When <code>true</code> any input files matching an output file glob pattern are included.</p>
</blockquote></td>
</tr>
</tbody>
</table>

{{% warning %}}

Breaking change: the `file` qualifier interprets `:` as path separator,
therefore `file 'foo:bar'` captures both files `foo` and `bar`. The
`path` qualifier interprets it just a plain file name character, and
therefore the output definition `path 'foo:bar'` captures the output
file with name `foo:bar`.

{{% /warning %}}

{{% tip %}}

The `path` qualifier should be preferred over `file` to handle process
output files when using Nextflow 19.10.0 or later.

</div>

### Output 'stdout' special file

The `stdout` qualifier allows you to <span
class="title-ref">capture</span> the <span
class="title-ref">stdout</span> output of the executed process and send
it over the channel specified in the output parameter declaration. For
example:

    process echoSomething {
        output:
        stdout channel

        """
        echo Hello world!
        """
    }

    channel.subscribe { print "I say..  $it" }

### Output 'env'

The `env` qualifier allows you to capture a variable defined in the
process execution environment and send it over the channel specified in
the output parameter declaration:

    process myTask {
        output:
        env FOO into target
        script:
        '''
        FOO=$(ls -la)
        '''
    }

    target.view { "directory content: $it" }

### Output 'set' of values

{{% warning %}}

The <span class="title-ref">set</span> output type has been deprecated.
See <span class="title-ref">tuple</span> instead.

{{% /warning %}}

### Output 'tuple' of values

The `tuple` qualifier allows to send multiple values into a single
channel. This feature is useful when you need to <span
class="title-ref">group together</span> the results of multiple
executions of the same process, as shown in the following example:

    query_ch = Channel.fromPath '*.fa'
    species_ch = Channel.from 'human', 'cow', 'horse'

    process blast {

    input:
      val species from query_ch
      file query from species_ch

    output:
      tuple val(species), file('result') into blastOuts

    script:
      """
      blast -db nr -query $query > result
      """
    }

In the above example a <span class="title-ref">BLAST</span> task is
executed for each pair of `species` and `query` that are received. When
the task completes a new tuple containing the value for `species` and
the file `result` is sent to the `blastOuts` channel.

A <span class="title-ref">tuple</span> declaration can contain any
combination of the following qualifiers, previously described: `val`,
`file` and `stdout`.

{{% tip %}}

Variable identifiers are interpreted as <span
class="title-ref">values</span> while strings literals are interpreted
as <span class="title-ref">files</span> by default, thus the above
output <span class="title-ref">tuple</span> can be rewritten using a
short notation as shown below.

</div>

    output:
        tuple species, 'result' into blastOuts

File names can be defined in a dynamic manner as explained in the
`process-dynoutname` section.

### Optional Output

In most cases a process is expected to generate output that is added to
the output channel. However, there are situations where it is valid for
a process to <span class="title-ref">not</span> generate output. In
these cases `optional true` may be added to the output declaration,
which tells Nextflow not to fail the process if the declared output is
not created.

    output:
        file("output.txt") optional true into outChannel

In this example, the process is normally expected to generate an
`output.txt` file, but in the cases where the file is legitimately
missing, the process does not fail. `outChannel` is only populated by
those processes that do generate `output.txt`.

When
----

The `when` declaration allows you to define a condition that must be
verified in order to execute the process. This can be any expression
that evaluates a boolean value.

It is useful to enable/disable the process execution depending the state
of various inputs and parameters. For example:

    process find {
      input:
      file proteins
      val type from dbtype

      when:
      proteins.name =~ /^BB11.*/ && type == 'nr'

      script:
      """
      blastp -query $proteins -db nr
      """

    }

Directives
----------

Using the <span class="title-ref">directive</span> declarations block
you can provide optional settings that will affect the execution of the
current process.

They must be entered at the top of the process <span
class="title-ref">body</span>, before any other declaration blocks (i.e.
`input`, `output`, etc) and have the following syntax:

    name value [, value2 [,..]]

Some directives are generally available to all processes, some others
depends on the <span class="title-ref">executor</span> currently
defined.

The directives are:

-   [accelerator](#accelerator)
-   [afterScript](#afterscript)
-   [beforeScript](#beforescript)
-   [cache](#cache)
-   [cpus](#cpus)
-   [conda](#conda)
-   [container](#container)
-   [containerOptions](#containeroptions)
-   [clusterOptions](#clusteroptions)
-   [disk](#disk)
-   [echo](#echo)
-   [errorStrategy](#errorstrategy)
-   [executor](#executor)
-   [ext](#ext)
-   [label](#label)
-   [machineType](#machinetype)
-   [maxErrors](#maxerrors)
-   [maxForks](#maxforks)
-   [maxRetries](#maxretries)
-   [memory](#memory)
-   [module](#module)
-   [penv](#penv)
-   [pod](#pod)
-   [publishDir](#publishdir)
-   [queue](#queue)
-   [scratch](#scratch)
-   [stageInMode](#stageinmode)
-   [stageOutMode](#stageoutmode)
-   [storeDir](#storedir)
-   [tag](#tag)
-   [time](#time)
-   [validExitStatus](#validexitstatus)

### accelerator

The `accelerator` directive allows you to specify the hardware
accelerator requirement for the task execution e.g. *GPU* processor. For
example:

    process foo {
        accelerator 4, type: 'nvidia-tesla-k80'

        script:
        """
        your_gpu_enabled --command --line
        """
    }

The above examples will request 4 GPUs of type <span
class="title-ref">nvidia-tesla-k80</span>.

{{% note %}}

This directive is only supported by `awsbatch-executor`,
`google-lifesciences-executor` and `k8s-executor` executors.

</div>

{{% tip %}}

The accelerator `type` option value depends by the target execution
platform. Refer to the target platform documentation for details on the
available accelerators.
[AWS](https://aws.amazon.com/batch/faqs/?#GPU_Scheduling_)
[Google](https://cloud.google.com/compute/docs/gpus/)
[Kubernetes](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/#clusters-containing-different-types-of-gpus).

</div>

### afterScript

The `afterScript` directive allows you to execute a custom (Bash)
snippet immediately *after* the main process has run. This may be useful
to clean up your staging area.

### beforeScript

The `beforeScript` directive allows you to execute a custom (Bash)
snippet *before* the main process script is run. This may be useful to
initialise the underlying cluster environment or for other custom
initialisation.

For example:

    process foo {

      beforeScript 'source /cluster/bin/setup'

      """
      echo bar
      """

    }

### cache

The `cache` directive allows you to store the process results to a local
cache. When the cache is enabled *and* the pipeline is launched with the
`resume <getstart-resume>` option, any following attempt to execute the
process, along with the same inputs, will cause the process execution to
be skipped, producing the stored data as the actual results.

The caching feature generates a unique <span
class="title-ref">key</span> by indexing the process script and inputs.
This key is used identify univocally the outputs produced by the process
execution.

The cache is enabled by default, you can disable it for a specific
process by setting the `cache` directive to `false`. For example:

    process noCacheThis {
      cache false

      script:
      <your command string here>
    }

The `cache` directive possible values are shown in the following table:

| Value            | Description                                                                                                                                                                                                                                                         |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `false`          | Disable cache feature.                                                                                                                                                                                                                                              |
| `true` (default) | Enable caching. Cache keys are created indexing input files meta-data information (name, size and last update timestamp attributes).                                                                                                                                |
| `'deep'`         | Enable caching. Cache keys are created indexing input files content.                                                                                                                                                                                                |
| `'lenient'`      | Enable caching. Cache keys are created indexing input files path and size attributes (this policy provides a workaround for incorrect caching invalidation observed on shared file systems due to inconsistent files timestamps; requires version 0.32.x or later). |

### conda

The `conda` directive allows for the definition of the process
dependencies using the [Conda](https://conda.io) package manager.

Nextflow automatically sets up an environment for the given package
names listed by in the `conda` directive. For example:

    process foo {
      conda 'bwa=0.7.15'

      '''
      your_command --here
      '''
    }

Multiple packages can be specified separating them with a blank space
eg. `bwa=0.7.15 fastqc=0.11.5`. The name of the channel from where a
specific package needs to be downloaded can be specified using the usual
Conda notation i.e. prefixing the package with the channel name as shown
here `bioconda::bwa=0.7.15`.

The `conda` directory also allows the specification of a Conda
environment file path or the path of an existing environment directory.
See the `conda-page` page for further details.

### container

The `container` directive allows you to execute the process script in a
[Docker](http://docker.io) container.

It requires the Docker daemon to be running in machine where the
pipeline is executed, i.e. the local machine when using the *local*
executor or the cluster nodes when the pipeline is deployed through a
*grid* executor.

For example:

    process runThisInDocker {

      container 'dockerbox:tag'

      """
      <your holy script here>
      """

    }

Simply replace in the above script `dockerbox:tag` with the Docker image
name you want to use.

{{% tip %}}

This can be very useful to execute your scripts into a replicable
self-contained environment or to deploy your pipeline in the cloud.

</div>

{{% note %}}

This directive is ignore for processes
`executed natively <process-native>`.

</div>

### containerOptions

The `containerOptions` directive allows you to specify any container
execution option supported by the underlying container engine (ie.
Docker, Singularity, etc). This can be useful to provide container
settings only for a specific process e.g. mount a custom path:

    process runThisWithDocker {

        container 'busybox:latest'
        containerOptions '--volume /data/db:/db'

        output: file 'output.txt'

        '''
        your_command --data /db > output.txt
        '''
    }

{{% warning %}}

This feature is not supported by `awsbatch-executor` and `k8s-executor`
executors.

{{% /warning %}}

### cpus

The `cpus` directive allows you to define the number of (logical) CPU
required by the process' task. For example:

    process big_job {

      cpus 8
      executor 'sge'

      """
      blastp -query input_sequence -num_threads ${task.cpus}
      """
    }

This directive is required for tasks that execute multi-process or
multi-threaded commands/tools and it is meant to reserve enough CPUs
when a pipeline task is executed through a cluster resource manager.

See also: [penv](#penv), [memory](#memory), [time](#time),
[queue](#queue), [maxForks](#maxforks)

### clusterOptions

The `clusterOptions` directive allows the usage of any <span
class="title-ref">native</span> configuration option accepted by your
cluster submit command. You can use it to request non-standard resources
or use settings that are specific to your cluster and not supported out
of the box by Nextflow.

{{% note %}}

This directive is taken in account only when using a grid based
executor: `sge-executor`, `lsf-executor`, `slurm-executor`,
`pbs-executor`, `pbspro-executor`, `moab-executor` and `condor-executor`
executors.

</div>

### disk

The `disk` directive allows you to define how much local disk storage
the process is allowed to use. For example:

    process big_job {

        disk '2 GB'
        executor 'cirrus'

        """
        your task script here
        """
    }

The following memory unit suffix can be used when specifying the disk
value:

| Unit | Description |
|------|-------------|
| B    | Bytes       |
| KB   | Kilobytes   |
| MB   | Megabytes   |
| GB   | Gigabytes   |
| TB   | Terabytes   |

{{% note %}}

This directive currently is taken in account only by the
`ignite-executor` and the `condor-executor` executors.

</div>

See also: [cpus](#cpus), [memory](#memory) [time](#time),
[queue](#queue) and [Dynamic computing
resources](#dynamic-computing-resources).

### echo

By default the <span class="title-ref">stdout</span> produced by the
commands executed in all processes is ignored. Setting the `echo`
directive to `true` you can forward the process <span
class="title-ref">stdout</span> to the current top running process <span
class="title-ref">stdout</span> file, showing it in the shell terminal.

For example:

    process sayHello {
      echo true

      script:
      "echo Hello"
    }

    Hello

Without specifying `echo true` you won't see the `Hello` string printed
out when executing the above example.

### errorStrategy

The `errorStrategy` directive allows you to define how an error
condition is managed by the process. By default when an error status is
returned by the executed script, the process stops immediately. This in
turn forces the entire pipeline to terminate.

Table of available error strategies:

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Executor</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>terminate</code></td>
<td><blockquote>
<p>Terminates the execution as soon as an error condition is reported. Pending jobs are killed (default)</p>
</blockquote></td>
</tr>
<tr class="even">
<td><code>finish</code></td>
<td><blockquote>
<p>Initiates an orderly pipeline shutdown when an error condition is raised, waiting the completion of any submitted job.</p>
</blockquote></td>
</tr>
<tr class="odd">
<td><code>ignore</code></td>
<td><blockquote>
<p>Ignores processes execution errors.</p>
</blockquote></td>
</tr>
<tr class="even">
<td><code>retry</code></td>
<td><blockquote>
<p>Re-submit for execution a process returning an error condition.</p>
</blockquote></td>
</tr>
</tbody>
</table>

When setting the `errorStrategy` directive to `ignore` the process
doesn't stop on an error condition, it just reports a message notifying
you of the error event.

For example:

    process ignoreAnyError {
       errorStrategy 'ignore'

       script:
       <your command string here>
    }

{{% tip %}}

By definition a command script fails when it ends with a non-zero exit
status. To change this behavior see [validExitStatus](#validexitstatus).

</div>

The `retry` <span class="title-ref">error strategy</span>, allows you to
re-submit for execution a process returning an error condition. For
example:

    process retryIfFail {
       errorStrategy 'retry'

       script:
       <your command string here>
    }

The number of times a failing process is re-executed is defined by the
[maxRetries](#maxretries) and [maxErrors](#maxerrors) directives.

{{% note %}}

More complex strategies depending on the task exit status or other
parametric values can be defined using a dynamic `errorStrategy`
directive. See the [Dynamic directives](#dynamic-directives) section for
details.

</div>

See also: [maxErrors](#maxerrors), [maxRetries](#maxretries) and
[Dynamic computing resources](#dynamic-computing-resources).

### executor

The <span class="title-ref">executor</span> defines the underlying
system where processes are executed. By default a process uses the
executor defined globally in the `nextflow.config` file.

The `executor` directive allows you to configure what executor has to be
used by the process, overriding the default configuration. The following
values can be used:

| Name               | Executor                                                                                                                       |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------|
| `local`            | The process is executed in the computer where <span class="title-ref">Nextflow</span> is launched.                             |
| `sge`              | The process is executed using the Sun Grid Engine / [Open Grid Engine](http://gridscheduler.sourceforge.net/).                 |
| `uge`              | The process is executed using the [Univa Grid Engine](https://en.wikipedia.org/wiki/Univa_Grid_Engine/) job scheduler.         |
| `lsf`              | The process is executed using the [Platform LSF](http://en.wikipedia.org/wiki/Platform_LSF) job scheduler.                     |
| `slurm`            | The process is executed using the SLURM job scheduler.                                                                         |
| `pbs`              | The process is executed using the [PBS/Torque](http://en.wikipedia.org/wiki/Portable_Batch_System) job scheduler.              |
| `pbspro`           | The process is executed using the [PBS Pro](https://www.pbsworks.com/) job scheduler.                                          |
| `moab`             | The process is executed using the [Moab](http://www.adaptivecomputing.com/moab-hpc-basic-edition/) job scheduler.              |
| `condor`           | The process is executed using the [HTCondor](https://research.cs.wisc.edu/htcondor/) job scheduler.                            |
| `nqsii`            | The process is executed using the [NQSII](https://www.rz.uni-kiel.de/en/our-portfolio/hiperf/nec-linux-cluster) job scheduler. |
| `ignite`           | The process is executed using the [Apache Ignite](https://ignite.apache.org/) cluster.                                         |
| `k8s`              | The process is executed using the [Kubernetes](https://kubernetes.io/) cluster.                                                |
| `awsbatch`         | The process is executed using the [AWS Batch](https://aws.amazon.com/batch/) service.                                          |
| `google-pipelines` | The process is executed using the [Google Genomics Pipelines](https://cloud.google.com/genomics/) service.                     |

The following example shows how to set the process's executor:

    process doSomething {

       executor 'sge'

       script:
       <your script here>

    }

{{% note %}}

Each executor provides its own set of configuration options that can set
be in the <span class="title-ref">directive</span> declarations block.
See `executor-page` section to read about specific executor directives.

</div>

### ext

The `ext` is a special directive used as *namespace* for user custom
process directives. This can be useful for advanced configuration
options. For example:

    process mapping {
      container "biocontainers/star:${task.ext.version}"

      input:
      file genome from genome_file
      set sampleId, file(reads) from reads_ch

      """
      STAR --genomeDir $genome --readFilesIn $reads
      """
    }

In the above example, the process uses a container whose version is
controlled by the `ext.version` property. This can be defined in the
`nextflow.config` file as shown below:

    process.ext.version = '2.5.3'

### machineType

The `machineType` can be used to specify a predefined Google Compute
Platform [machine
type](https://cloud.google.com/compute/docs/machine-types) when running
using the `Google Pipeline <google-pipelines>` executor.

This directive is optional and if specified overrides the cpus and
memory directives:

    process foo {
      machineType 'n1-highmem-8'

      """
      <your script here>
      """
    }

{{% note %}}

This feature requires Nextflow 19.07.0 or later.

</div>

See also: [cpus](#cpus) and [memory](#memory).

### maxErrors

The `maxErrors` directive allows you to specify the maximum number of
times a process can fail when using the `retry` <span
class="title-ref">error strategy</span>. By default this directive is
disabled, you can set it as shown in the example below:

    process retryIfFail {
      errorStrategy 'retry'
      maxErrors 5

      """
      echo 'do this as that .. '
      """
    }

{{% note %}}

This setting considers the **total** errors accumulated for a given
process, across all instances. If you want to control the number of
times a process **instance** (aka task) can fail, use `maxRetries`.

</div>

See also: [errorStrategy](#errorstrategy) and [maxRetries](#maxretries).

### maxForks

The `maxForks` directive allows you to define the maximum number of
process instances that can be executed in parallel. By default this
value is equals to the number of CPU cores available minus 1.

If you want to execute a process in a sequential manner, set this
directive to one. For example:

    process doNotParallelizeIt {

       maxForks 1

       '''
       <your script here>
       '''

    }

### maxRetries

The `maxRetries` directive allows you to define the maximum number of
times a process instance can be re-submitted in case of failure. This
value is applied only when using the `retry` <span
class="title-ref">error strategy</span>. By default only one retry is
allowed, you can increase this value as shown below:

    process retryIfFail {
        errorStrategy 'retry'
        maxRetries 3

        """
        echo 'do this as that .. '
        """
    }

{{% note %}}

There is a subtle but important difference between `maxRetries` and the
`maxErrors` directive. The latter defines the total number of errors
that are allowed during the process execution (the same process can
launch different execution instances), while the `maxRetries` defines
the maximum number of times the same process execution can be retried in
case of an error.

</div>

See also: [errorStrategy](#errorstrategy) and [maxErrors](#maxerrors).

### memory

The `memory` directive allows you to define how much memory the process
is allowed to use. For example:

    process big_job {

        memory '2 GB'
        executor 'sge'

        """
        your task script here
        """
    }

The following memory unit suffix can be used when specifying the memory
value:

| Unit | Description |
|------|-------------|
| B    | Bytes       |
| KB   | Kilobytes   |
| MB   | Megabytes   |
| GB   | Gigabytes   |
| TB   | Terabytes   |

See also: [cpus](#cpus), [time](#time), [queue](#queue) and [Dynamic
computing resources](#dynamic-computing-resources).

### module

[Environment Modules](http://modules.sourceforge.net/) is a package
manager that allows you to dynamically configure your execution
environment and easily switch between multiple versions of the same
software tool.

If it is available in your system you can use it with Nextflow in order
to configure the processes execution environment in your pipeline.

In a process definition you can use the `module` directive to load a
specific module version to be used in the process execution environment.
For example:

    process basicExample {

      module 'ncbi-blast/2.2.27'

      """
      blastp -query <etc..>
      """
    }

You can repeat the `module` directive for each module you need to load.
Alternatively multiple modules can be specified in a single `module`
directive by separating all the module names by using a `:` (colon)
character as shown below:

    process manyModules {

      module 'ncbi-blast/2.2.27:t_coffee/10.0:clustalw/2.1'

      """
      blastp -query <etc..>
      """
    }

### penv

The `penv` directive allows you to define the <span
class="title-ref">parallel environment</span> to be used when submitting
a parallel task to the `SGE <sge-executor>` resource manager. For
example:

    process big_job {

      cpus 4
      penv 'smp'
      executor 'sge'

      """
      blastp -query input_sequence -num_threads ${task.cpus}
      """
    }

This configuration depends on the parallel environment provided by your
grid engine installation. Refer to your cluster documentation or contact
your admin to lean more about this.

{{% note %}}

This setting is available when using the `sge-executor` executor.

</div>

See also: [cpus](#cpus), [memory](#memory), [time](#time)

### pod

The `pod` directive allows the definition of pods specific settings,
such as environment variables, secrets and config maps when using the
`k8s-executor` executor.

For example:

    process your_task {
      pod env: 'FOO', value: 'bar'

      '''
      echo $FOO
      '''
    }

The above snippet defines an environment variable named `FOO` which
value is `bar`.

The `pod` directive allows the definition of the following options:

|                                                 |                                                                                                                                                                                                                                                                                                                                                              |
|-------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `label: <K>, value: <V>`                        | Defines a pod label with key `K` and value `V`.                                                                                                                                                                                                                                                                                                              |
| `annotation: <K>, value: <V>`                   | Defines a pod annotation with key `K` and value `V`.                                                                                                                                                                                                                                                                                                         |
| `env: <E>, value: <V>`                          | Defines an environment variable with name `E` and whose value is given by the `V` string.                                                                                                                                                                                                                                                                    |
| `env: <E>, config: <C/K>`                       | Defines an environment variable with name `E` and whose value is given by the entry associated to the key with name `K` in the [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) with name `C`.                                                                                                                 |
| `env: <E>, secret: <S/K>`                       | Defines an environment variable with name `E` and whose value is given by the entry associated to the key with name `K` in the [Secret](https://kubernetes.io/docs/concepts/configuration/secret/) with name `S`.                                                                                                                                            |
| `config: <C/K>, mountPath: </absolute/path>`    | The content of the [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) with name `C` with key `K` is made available to the path `/absolute/path`. When the key component is omitted the path is interpreted as a directory and all the <span class="title-ref">ConfigMap</span> entries are exposed in that path. |
| `secret: <S/K>, mountPath: </absolute/path>`    | The content of the [Secret](https://kubernetes.io/docs/concepts/configuration/secret/) with name `S` with key `K` is made available to the path `/absolute/path`. When the key component is omitted the path is interpreted as a directory and all the <span class="title-ref">Secret</span> entries are exposed in that path.                               |
| `volumeClaim: <V>, mountPath: </absolute/path>` | Mounts a [Persistent volume claim](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) with name `V` to the specified path location. Use the optional <span class="title-ref">subPath</span> parameter to mount a directory inside the referenced volume instead of its root.                                                                   |
| `imagePullPolicy: <V>`                          | Specifies the strategy to be used to pull the container image e.g. `imagePullPolicy: 'Always'`.                                                                                                                                                                                                                                                              |
| `imagePullSecret: <V>`                          | Specifies the secret name to access a private container image registry. See [Kubernetes documentation](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod) for details.                                                                                                                                             |
| `runAsUser: <UID>`                              | Specifies the user ID to be used to run the container.                                                                                                                                                                                                                                                                                                       |
| `nodeSelector: <V>`                             | Specifies which node the process will run on. See [Kubernetes nodeSelector](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#nodeselector) for details.                                                                                                                                                                                    |

When defined in the Nextflow configuration file, a pod setting can be
defined using the canonical associative array syntax. For example:

    process {
      pod = [env: 'FOO', value: 'bar']
    }

When more than one setting needs to be provides they must be enclosed in
a list definition as shown below:

    process {
      pod = [ [env: 'FOO', value: 'bar'], [secret: 'my-secret/key1', mountPath: '/etc/file.txt'] ]
    }

### publishDir

The `publishDir` directive allows you to publish the process output
files to a specified folder. For example:

    process foo {

        publishDir '/data/chunks'

        output:
        file 'chunk_*' into letters

        '''
        printf 'Hola' | split -b 1 - chunk_
        '''
    }

The above example splits the string `Hola` into file chunks of a single
byte. When complete the `chunk_*` output files are published into the
`/data/chunks` folder.

{{% tip %}}

The `publishDir` directive can be specified more than one time in to
publish the output files to different target directories. This feature
requires version 0.29.0 or higher.

</div>

By default files are published to the target folder creating a *symbolic
link* for each process output that links the file produced into the
process working directory. This behavior can be modified using the
`mode` parameter.

Table of optional parameters that can be used with the `publishDir`
directive:

| Name      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mode      | The file publishing method. See the following table for possible values.                                                                                                                                                                                                                                                                                                                                                                                |
| overwrite | When `true` any existing file in the specified folder will be overridden (default: `true` during normal pipeline execution and `false` when pipeline execution is <span class="title-ref">resumed</span>).                                                                                                                                                                                                                                              |
| pattern   | Specifies a [glob](http://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob) file pattern that selects which files to publish from the overall set of output files.                                                                                                                                                                                                                                                                        |
| path      | Specifies the directory where files need to be published. **Note**: the syntax `publishDir '/some/dir'` is a shortcut for `publishDir path: '/some/dir'`.                                                                                                                                                                                                                                                                                               |
| saveAs    | A closure which, given the name of the file being published, returns the actual file name or a full path where the file is required to be stored. This can be used to rename or change the destination directory of the published files dynamically by using a custom strategy. Return the value `null` from the closure to *not* publish a file. This is useful when the process has multiple output files, but you want to publish only some of them. |
| enabled   | Allow to enable or disable the publish rule depending the boolean value specified (default: `true`).                                                                                                                                                                                                                                                                                                                                                    |

Table of publish modes:

| Mode         | Description                                                                                                                                                                                                                           |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| symlink      | Creates an absolute <span class="title-ref">symbolic link</span> in the published directory for each process output file (default).                                                                                                   |
| rellink      | Creates a relative <span class="title-ref">symbolic link</span> in the published directory for each process output file.                                                                                                              |
| link         | Creates a <span class="title-ref">hard link</span> in the published directory for each process output file.                                                                                                                           |
| copy         | Copies the output files into the published directory.                                                                                                                                                                                 |
| copyNoFollow | Copies the output files into the published directory without following symlinks ie. copies the links themselves.                                                                                                                      |
| move         | Moves the output files into the published directory. **Note**: this is only supposed to be used for a <span class="title-ref">terminating</span> process i.e. a process whose output is not consumed by any other downstream process. |

{{% note %}}

The <span class="title-ref">mode</span> value needs to be specified as a
string literal i.e. enclosed by quote characters. Multiple parameters
need to be separated by a colon character. For example:

</div>

    process foo {

        publishDir '/data/chunks', mode: 'copy', overwrite: false

        output:
        file 'chunk_*' into letters

        '''
        printf 'Hola' | split -b 1 - chunk_
        '''
    }

{{% warning %}}

Files are copied into the specified directory in an *asynchronous*
manner, thus they may not be immediately available in the published
directory at the end of the process execution. For this reason files
published by a process must not be accessed by other downstream
processes.

{{% /warning %}}

### queue

The `queue` directory allows you to set the <span
class="title-ref">queue</span> where jobs are scheduled when using a
grid based executor in your pipeline. For example:

    process grid_job {

        queue 'long'
        executor 'sge'

        """
        your task script here
        """
    }

Multiple queues can be specified by separating their names with a comma
for example:

    process grid_job {

        queue 'short,long,cn-el6'
        executor 'sge'

        """
        your task script here
        """
    }

{{% note %}}

This directive is taken in account only by the following executors:
`sge-executor`, `lsf-executor`, `slurm-executor` and `pbs-executor`
executors.

</div>

### label

The `label` directive allows the annotation of processes with mnemonic
identifier of your choice. For example:

    process bigTask {

      label 'big_mem'

      '''
      <task script>
      '''
    }

The same label can be applied to more than a process and multiple labels
can be applied to the same process using the `label` directive more than
one time.

{{% note %}}

A label must consist of alphanumeric characters or `_`, must start with
an alphabetic character and must end with an alphanumeric character.

</div>

Labels are useful to organise workflow processes in separate groups
which can be referenced in the configuration file to select and
configure subset of processes having similar computing requirements.

See the `config-process-selectors` documentation for details.

### scratch

The `scratch` directive allows you to execute the process in a temporary
folder that is local to the execution node.

This is useful when your pipeline is launched by using a <span
class="title-ref">grid</span> executor, because it permits to decrease
the NFS overhead by running the pipeline processes in a temporary
directory in the local disk of the actual execution node. Only the files
declared as output in the process definition will be copied in the
pipeline working area.

In its basic form simply specify `true` at the directive value, as shown
below:

    process simpleTask {

      scratch true

      output:
      file 'data_out'

      '''
      <task script>
      '''
    }

By doing this, it tries to execute the script in the directory defined
by the variable `$TMPDIR` in the execution node. If this variable does
not exist, it will create a new temporary directory by using the Linux
command `mktemp`.

A custom environment variable, other than `$TMPDIR`, can be specified by
simply using it as the scratch value, for example:

    scratch '$MY_GRID_TMP'

Note, it must be wrapped by single quotation characters, otherwise the
variable will be evaluated in the pipeline script context.

You can also provide a specific folder path as scratch value, for
example:

    scratch '/tmp/my/path'

By doing this, a new temporary directory will be created in the
specified path each time a process is executed.

Finally, when the `ram-disk` string is provided as `scratch` value, the
process will be execute in the node RAM virtual disk.

Summary of allowed values:

| scratch   | Description                                                                                                                                          |
|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| false     | Do not use the scratch folder.                                                                                                                       |
| true      | Creates a scratch folder in the directory defined by the `$TMPDIR` variable; fallback to `mktemp /tmp` if that variable do not exists.               |
| $YOUR_VAR | Creates a scratch folder in the directory defined by the `$YOUR_VAR` environment variable; fallback to `mktemp /tmp` if that variable do not exists. |
| /my/tmp   | Creates a scratch folder in the specified directory.                                                                                                 |
| ram-disk  | Creates a scratch folder in the RAM disk `/dev/shm/` (experimental).                                                                                 |

### storeDir

The `storeDir` directive allows you to define a directory that is used
as <span class="title-ref">permanent</span> cache for your process
results.

In more detail, it affects the process execution in two main ways:

1.  The process is executed only if the files declared in the <span
    class="title-ref">output</span> clause do not exist in the directory
    specified by the `storeDir` directive. When the files exist the
    process execution is skipped and these files are used as the actual
    process result.
2.  Whenever a process is successfully completed the files listed in the
    <span class="title-ref">output</span> declaration block are moved
    into the directory specified by the `storeDir` directive.

The following example shows how to use the `storeDir` directive to
create a directory containing a BLAST database for each species
specified by an input parameter:

    genomes = Channel.fromPath(params.genomes)

    process formatBlastDatabases {

      storeDir '/db/genomes'

      input:
      file species from genomes

      output:
      file "${dbName}.*" into blastDb

      script:
      dbName = species.baseName
      """
      makeblastdb -dbtype nucl -in ${species} -out ${dbName}
      """

    }

{{% warning %}}

The `storeDir` directive is meant for long term process caching and
should not be used to output the files produced by a process to a
specific folder or organise result data in <span
class="title-ref">semantic</span> directory structure. In these cases
you may use the [publishDir](#publishdir) directive instead.

{{% /warning %}}

{{% note %}}

The use of AWS S3 path is supported however it requires the installation
of the [AWS CLI tool](https://aws.amazon.com/cli/) (i.e. `aws`) in the
target computing node.

</div>

### stageInMode

The `stageInMode` directive defines how input files are staged-in to the
process work directory. The following values are allowed:

| Value   | Description                                                                                                                        |
|---------|------------------------------------------------------------------------------------------------------------------------------------|
| copy    | Input files are staged in the process work directory by creating a copy.                                                           |
| link    | Input files are staged in the process work directory by creating an (hard) link for each of them.                                  |
| symlink | Input files are staged in the process work directory by creating a symbolic link with an absolute path for each of them (default). |
| rellink | Input files are staged in the process work directory by creating a symbolic link with a relative path for each of them.            |

### stageOutMode

The `stageOutMode` directive defines how output files are staged-out
from the scratch directory to the process work directory. The following
values are allowed:

| Value | Description                                                                                            |
|-------|--------------------------------------------------------------------------------------------------------|
| copy  | Output files are copied from the scratch directory to the work directory.                              |
| move  | Output files are moved from the scratch directory to the work directory.                               |
| rsync | Output files are copied from the scratch directory to the work directory by using the `rsync` utility. |

See also: [scratch](#scratch).

### tag

The `tag` directive allows you to associate each process executions with
a custom label, so that it will be easier to identify them in the log
file or in the trace execution report. For example:

    process foo {
      tag "$code"

      input:
      val code from 'alpha', 'gamma', 'omega'

      """
      echo $code
      """
    }

The above snippet will print a log similar to the following one, where
process names contain the tag value:

    [6e/28919b] Submitted process > foo (alpha)
    [d2/1c6175] Submitted process > foo (gamma)
    [1c/3ef220] Submitted process > foo (omega)

See also `Trace execution report <trace-report>`

### time

The `time` directive allows you to define how long a process is allowed
to run. For example:

    process big_job {

        time '1h'

        """
        your task script here
        """
    }

The following time unit suffix can be used when specifying the duration
value:

| Unit | Description |
|------|-------------|
| s    | Seconds     |
| m    | Minutes     |
| h    | Hours       |
| d    | Days        |

{{% note %}}

This directive is taken in account only when using one of the following
grid based executors: `sge-executor`, `lsf-executor`, `slurm-executor`,
`pbs-executor`, `condor-executor` and `awsbatch-executor` executors.

</div>

See also: [cpus](#cpus), [memory](#memory), [queue](#queue) and [Dynamic
computing resources](#dynamic-computing-resources).

### validExitStatus

{{% warning %}}

This feature has been deprecated and will be removed in a future
release.

{{% /warning %}}

A process is terminated when the executed command returns an error exit
status. By default any error status other than `0` is interpreted as an
error condition.

The `validExitStatus` directive allows you to fine control which error
status will represent a successful command execution. You can specify a
single value or multiple values as shown in the following example:

    process returnOk {
        validExitStatus 0,1,2

         script:
         """
         echo Hello
         exit 1
         """
    }

In the above example, although the command script ends with a `1` exit
status, the process will not return an error condition because the value
`1` is declared as a <span class="title-ref">valid</span> status in the
`validExitStatus` directive.

### Dynamic directives

A directive can be assigned *dynamically*, during the process execution,
so that its actual value can be evaluated depending on the value of one,
or more, process' input values.

In order to be defined in a dynamic manner the directive's value needs
to be expressed by using a `closure <script-closure>` statement, as in
the following example:

    process foo {

      executor 'sge'
      queue { entries > 100 ? 'long' : 'short' }

      input:
      set entries, file(x) from data

      script:
      """
      < your job here >
      """
    }

In the above example the [queue](#queue) directive is evaluated
dynamically, depending on the input value `entries`. When it is bigger
than 100, jobs will be submitted to the queue `long`, otherwise the
`short` one will be used.

All directives can be assigned to a dynamic value except the following:

-   [executor](#executor)
-   [maxForks](#maxforks)

{{% note %}}

You can retrieve the current value of a dynamic directive in the process
script by using the implicit variable `task` which holds the directive
values defined in the current process instance.

</div>

For example:

    process foo {

       queue { entries > 100 ? 'long' : 'short' }

       input:
       set entries, file(x) from data

       script:
       """
       echo Current queue: ${task.queue}
       """
     }

### Dynamic computing resources

It's a very common scenario that different instances of the same process
may have very different needs in terms of computing resources. In such
situations requesting, for example, an amount of memory too low will
cause some tasks to fail. Instead, using a higher limit that fits all
the tasks in your execution could significantly decrease the execution
priority of your jobs.

The [Dynamic directives](#dynamic-directives) evaluation feature can be
used to modify the amount of computing resources requested in case of a
process failure and try to re-execute it using a higher limit. For
example:

    process foo {

        memory { 2.GB * task.attempt }
        time { 1.hour * task.attempt }

        errorStrategy { task.exitStatus in 137..140 ? 'retry' : 'terminate' }
        maxRetries 3

        script:
        <your job here>

    }

In the above example the [memory](#memory) and execution [time](#time)
limits are defined dynamically. The first time the process is executed
the `task.attempt` is set to `1`, thus it will request a two GB of
memory and one hour of maximum execution time.

If the task execution fail reporting an exit status in the range between
137 and 140, the task is re-submitted (otherwise terminates
immediately). This time the value of `task.attempt` is `2`, thus
increasing the amount of the memory to four GB and the time to 2 hours,
and so on.

The directive [maxRetries](#maxretries) set the maximum number of time
the same task can be re-executed.

### Dynamic Retry with backoff

There are cases in which the required execution resources may be
temporary unavailable e.g. network congestion. In these cases
immediately re-executing the task will likely result in the identical
error. A retry with an exponential backoff delay can better recover
these error conditions:

    process foo {
      errorStrategy { sleep(Math.pow(2, task.attempt) * 200 as long); return 'retry' }
      maxRetries 5
      script:
      '''
      your_command --here
      '''
    }
