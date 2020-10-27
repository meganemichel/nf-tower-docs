---
title: Language basics
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
    weight: 2
---

## Hello world

To print something is as easy as using one of the `print` or `println` methods. :

{{< highlight groovy >}}
    println "Hello, World!"
{{< /highlight >}}

The only difference between the two is that the `println` method implicitly appends a _new line_ character to the printed string.

## Variables

To define a variable, simply assign a value to it:

{{< highlight groovy >}}
    x = 1
    println x

    x = new java.util.Date()
    println x

    x = -3.1499392
    println x

    x = false
    println x

    x = "Hi"
    println x
{{< /highlight >}}

## Lists

A List object can be defined by placing the list items in square brackets:

{{< highlight groovy >}}
    myList = [1776, -1, 33, 99, 0, 928734928763]
{{< /highlight >}}

You can access a given item in the list with square-bracket notation (indexes start at 0):

{{< highlight groovy >}}
    println myList[0]
{{< /highlight >}}

In order to get the length of the list use the `size` method:

{{< highlight groovy >}}
    println myList.size()
{{< /highlight >}}

Learn more about lists:

-   [Groovy Lists tutorial](http://groovy-lang.org/groovy-dev-kit.html#Collections-Lists)
-   [Groovy List SDK](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/util/List.html)
-   [Java List SDK](http://docs.oracle.com/javase/7/docs/api/java/util/List.html)

## Maps

Maps are used to store _associative arrays_ or _dictionaries_. They are unordered collections of heterogeneous, named data:

{{< highlight groovy >}}
    scores = [ "Brett":100, "Pete":"Did not finish", "Andrew":86.87934 ]
{{< /highlight >}}

Note that each of the values stored in the map can be of a different type. `Brett` is an integer, `Pete` is a string, and `Andrew` is a floating-point number.

We can access the values in a map in two main ways:

{{< highlight groovy >}}
    println scores["Pete"]
    println scores.Pete
{{< /highlight >}}

To add data to or modify a map, the syntax is similar to adding values to list:

{{< highlight groovy >}}
    scores["Pete"] = 3
    scores["Cedric"] = 120
{{< /highlight >}}

Learn more about maps:

-   [Groovy Maps tutorial](http://groovy-lang.org/groovy-dev-kit.html#Collections-Maps)
-   [Groovy Map SDK](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/util/Map.html)
-   [Java Map SDK](http://docs.oracle.com/javase/7/docs/api/java/util/Map.html)

## Multiple assignment

An array or a list object can used to assign to multiple variables at once:

{{< highlight groovy >}}
    (a, b, c) = [10, 20, 'foo']
    assert a == 10 && b == 20 && c == 'foo'
{{< /highlight >}}

The three variables on the left of the aj the list.

Read more about [Multiple assignment](http://www.groovy-lang.org/semantics.html#_multiple_assignment) in the Groovy documentation.

## Conditional Execution

One of the most important features of any programming language is the ability to execute different code under different conditions. The simplest way to do this is to use the `if` construct:

{{< highlight groovy >}}
    x = Math.random()
    if( x < 0.5 ) {
        println "You lost."
    }
    else {
        println "You won!"
    }
{{< /highlight >}}

## Strings

Strings can be defined by enclosing text in single or double quotes (`'` or `"` characters):

{{< highlight groovy >}}
    println "he said 'cheese' once"
    println 'he said "cheese!" again'
{{< /highlight >}}

Strings can be concatenated with `+`:

{{< highlight groovy >}}
    a = "world"
    print "hello " + a + "\n"
{{< /highlight >}}

## String interpolation

There is an important difference between single-quoted and double-quoted strings: Double-quoted strings support variable interpolations, while single-quoted strings do not.

In practice, double-quoted strings can contain the value of an arbitrary variable by prefixing its name with the `$` character, or the value of any expression by using the `${expression}` syntax, similar to Bash/shell scripts:

{{< highlight groovy >}}
    foxtype = 'quick'
    foxcolor = ['b', 'r', 'o', 'w', 'n']
    println "The $foxtype ${foxcolor.join()} fox"

    x = 'Hello'
    println '$x + $y'
{{< /highlight >}}

This code prints:

{{< highlight groovy >}}
    The quick brown fox
    $x + $y
{{< /highlight >}}

## Multi-line strings

A block of text that span multiple lines can be defined by delimiting it with triple single or double quotes:

{{< highlight groovy >}}
    text = """
        hello there James
        how are you today?
        """
{{< /highlight >}}

{{% note %}}
Like before, multi-line strings inside double quotes support variable interpolation, while single-quoted multi-line strings do not.

{{% /note %}}

As in Bash/shell scripts, terminating a line in a multi-line string with a `\` character prevents a a _new line_ character from separating that line from the one that follows:

{{< highlight groovy >}}
    myLongCmdline = """ blastp \
                    -in $input_query \
                    -out $output_file \
                    -db $blast_database \
                    -html
                    """

    result = myLongCmdline.execute().text
{{< / highlight >}}

In the preceding example, `blastp` and its `-in`, `-out`, `-db` and `-html` switches and their arguments are effectively a single line.
