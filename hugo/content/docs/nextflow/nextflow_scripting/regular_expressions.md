---
title: Regular expressions
aliases:
- "docs/latest/script.html#regular-expressions"
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
    weight: 5
---

Regular expressions are the Swiss Army knife of text processing. They provide the programmer with the ability to match and extract patterns from strings.

Regular expressions are available via the `~/pattern/` syntax and the `=~` and `==~` operators.

Use `=~` to check whether a given pattern occurs anywhere in a string:

{{< highlight groovy >}}
    assert 'foo' =~ /foo/       // return TRUE
    assert 'foobar' =~ /foo/    // return TRUE
{{< /highlight >}}

Use `==~` to check whether a string matches a given regular expression pattern exactly. :

{{< highlight groovy >}}
    assert 'foo' ==~ /foo/       // return TRUE
    assert 'foobar' ==~ /foo/    // return FALSE
{{< /highlight >}}

It is worth noting that the `~` operator creates a Java `Pattern` object from the given string, while the `=~` operator creates a Java `Matcher` object. :

{{< highlight groovy >}}
    x = ~/abc/
    println x.class
    // prints java.util.regex.Pattern

    y = 'some string' =~ /abc/
    println y.class
    // prints java.util.regex.Matcher
{{< /highlight >}}

Regular expression support is imported from Java. Java's regular expression language and API is documented in the [Pattern Java documentation](http://download.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html).

You may also be interested in this post: [Groovy: Don't Fear the RegExp](https://web.archive.org/web/20170621185113/http://www.naleid.com/blog/2008/05/19/dont-fear-the-regexp).

## String replacement

To replace pattern occurrences in a given string, use the `replaceFirst` and `replaceAll` methods:

{{< highlight groovy >}}
    x = "colour".replaceFirst(/ou/, "o")
    println x
    // prints: color

    y = "cheesecheese".replaceAll(/cheese/, "nice")
    println y
    // prints: nicenice
{{< /highlight >}}

## Capturing groups

You can match a pattern that includes groups. First create a matcher object with the `=~` operator. Then, you can index the matcher object to find the matches: `matcher[0]` returns a list representing the first match of the regular expression in the string. The first list element is the string that matches the entire regular expression, and the remaining elements are the strings that match each group.

Here's how it works:

{{< highlight groovy >}}
    programVersion = '2.7.3-beta'
    m = programVersion =~ /(\d+)\.(\d+)\.(\d+)-?(.+)/

    assert m[0] ==  ['2.7.3-beta', '2', '7', '3', 'beta']
    assert m[0][1] == '2'
    assert m[0][2] == '7'
    assert m[0][3] == '3'
    assert m[0][4] == 'beta'
{{< /highlight >}}

Applying some syntactic sugar, you can do the same in just one line of code:

{{< highlight groovy >}}
    programVersion = '2.7.3-beta'
    (full, major, minor, patch, flavor) = (programVersion =~ /(\d+)\.(\d+)\.(\d+)-?(.+)/)[0]

    println full    // 2.7.3-beta
    println major   // 2
    println minor   // 7
    println patch   // 3
    println flavor  // beta
{{< /highlight >}}

## Removing part of a string

You can remove part of a `String` value using a regular expression pattern. The first match found is replaced with an empty String:

{{< highlight groovy >}}
    // define the regexp pattern
    wordStartsWithGr = ~/(?i)\s+Gr\w+/

    // apply and verify the result
    ('Hello Groovy world!' - wordStartsWithGr) == 'Hello world!'
    ('Hi Grails users' - wordStartsWithGr) == 'Hi users'
{{< /highlight >}}

Remove the first 5-character word from a string:

{{< highlight groovy >}}
    assert ('Remove first match of 5 letter word' - ~/\b\w{5}\b/) == 'Remove  match of 5 letter word'
{{< /highlight >}}

Remove the first number with its trailing whitespace from a string:

{{< highlight groovy >}}
    assert ('Line contains 20 characters' - ~/\d+\s+/) == 'Line contains characters'
{{< /highlight >}}
