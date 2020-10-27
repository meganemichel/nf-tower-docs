---
title: Closures
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
    weight: 4
---

Briefly, a closure is a block of code that can be passed as an argument to a function. Thus, you can define a chunk of code and then pass it around as if it were a string or an integer.

More formally, you can create functions that are defined as _first class objects_.

{{< highlight groovy >}}
    square = { it * it }
{{< /highlight >}}

The curly brackets around the expression `it * it` tells the script interpreter to treat this expression as code. The _it_ identifier is an implicit variable that represents the value that is passed to the function when it is invoked.

Once compiled the function object is assigned to the variable `square` as any other variable assignments shown previously. Now we can do something like this:

{{< highlight groovy >}}
    println square(9)
{{< /highlight >}}

and get the value 81.

This is not very interesting until we find that we can pass the function `square` as an argument to other functions or methods. Some built-in functions take a function like this as an argument. One example is the `collect` method on lists:
{{< highlight groovy >}}
    [ 1, 2, 3, 4 ].collect(square)
{{< /highlight >}}

This expression says: Create an array with the values 1, 2, 3 and 4, then call its `collect` method, passing in the closure we defined above. The `collect` method runs through each item in the array, calls the closure on the item, then puts the result in a new array, resulting in:

{{< highlight groovy >}}
    [ 1, 4, 9, 16 ]
{{< /highlight >}}

For more methods that you can call with closures as arguments, see the [Groovy GDK documentation](http://docs.groovy-lang.org/latest/html/groovy-jdk/).

By default, closures take a single parameter called `it`, but you can also create closures with multiple, custom-named parameters. For example, the method `Map.each()` can take a closure with two arguments, to which it binds the _key_ and the associated _value_ for each key-value pair in the `Map`. Here, we use the obvious variable names `key` and `value` in our closure:

{{< highlight groovy >}}
    printMapClosure = { key, value ->
        println "$key = $value"
    }

    [ "Yue" : "Wu", "Mark" : "Williams", "Sudha" : "Kumari" ].each(printMapClosure)
{{< /highlight >}}

Prints:

{{< highlight groovy >}}
    Yue=Wu
    Mark=Williams
    Sudha=Kumari
{{< /highlight >}}

A closure has two other important features. First, it can access variables in the scope where it is defined, so that it can _interact_ with them.

Second, a closure can be defined in an _anonymous_ manner, meaning that it is not given a name, and is defined in the place where it needs to be used.

As an example showing both these features, see the following code fragment:

{{< highlight groovy >}}
    myMap = ["China": 1 , "India" : 2, "USA" : 3]

    result = 0
    myMap.keySet().each( { result+= myMap[it] } )

    println result
{{< /highlight >}}

Learn more about closures in the [Groovy documentation](http://groovy-lang.org/closures.html)
