---
title: Files and I/O
aliases:
- "/docs/nextflow-scripting/files-and-i-o"
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
    weight: 6
---

To access and work with files, use the `file` method, which returns a file system object given a file path string:

{{< highlight groovy >}}
    myFile = file('some/path/to/my_file.file')
{{< /highlight >}}

The `file` method can reference either _files</span> or <span class="title-ref">directories_, depending on what the string path refers to in the file system.

When using the wildcard characters `✽`, `?`, `[]` and `{}`, the argument is interpreted as a [glob](http://docs.oracle.com/javase/tutorial/essential/io/fileOps.html#glob) path matcher and the `file` method returns a list object holding the paths of files whose names match the specified pattern, or an empty list if no match is found:

{{< highlight groovy >}}
    listOfFiles = file('some/path/*.fa')
{{< /highlight >}}

{{% note %}}

Two asterisks (`✽✽`) in a glob pattern works like `✽` but matches any number of directory components in a file system path.

{{% /note %}}

By default, wildcard characters do not match directories or hidden files. For example, if you want to include hidden files in the result list, add the optional parameter `hidden`:

{{< highlight groovy >}}
    listWithHidden = file('some/path/*.fa', hidden: true)
{{< /highlight >}}

Here are `file`'s available options:

| Name          | Description                                                                                                                                |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| glob          | When `true` interprets characters `*`, `?`, `[]` and `{}` as glob wildcards, otherwise handles them as normal characters (default: `true`) |
| type          | Type of paths returned, either `file`, `dir` or `any` (default: `file`)                                                                    |
| hidden        | When `true` includes hidden files in the resulting paths (default: `false`)                                                                |
| maxDepth      | Maximum number of directory levels to visit (default: _no limit_)                                             |
| followLinks   | When `true` follows symbolic links during directory tree traversal, otherwise treats them as files (default: `true`)                       |
| checkIfExists | When `true` throws an exception of the specified path do not exist in the file system (default: `false`)                                   |

{{% tip %}}

If you are a Java geek you will be interested to know that the `file` method returns a [Path](http://docs.oracle.com/javase/8/docs/api/java/nio/file/Path.html) object, which allows you to use the usual methods you would in a Java program.

{{% /tip %}}

See also: `Channel.fromPath <channel-path>`.

## Basic read/write

Given a file variable, declared using the `file` method as shown in the previous example, reading a file is as easy as getting the value of the file's `text` property, which returns the file content as a string value:

{{< highlight groovy >}}
    print myFile.text
{{< /highlight >}}

Similarly, you can save a string value to a file by simply assigning it to the file's `text` property:

{{< highlight groovy >}}
    myFile.text = 'Hello world!'
{{< /highlight >}}

{{% note %}}

Existing file content is overwritten by the assignment operation, which also implicitly creates files that do not exist.

{{% /note %}}

In order to append a string value to a file without erasing existing content, you can use the `append` method:

{{< highlight groovy >}}
    myFile.append('Add this line\n')
{{< /highlight >}}

Or use the _left shift_ operator, a more idiomatic way to append text content to a file:

{{< highlight groovy >}}
    myFile << 'Add a line more\n'
{{< /highlight >}}

Binary data can managed in the same way, just using the file property `bytes` instead of `text`. Thus, the following example reads the file and returns its content as a byte array:

{{< highlight groovy >}}
    binaryContent = myFile.bytes
{{< /highlight >}}

Or you can save a byte array data buffer to a file, by simply writing:

{{< highlight groovy >}}
    myFile.bytes = binaryBuffer
{{< /highlight >}}

{{% warning %}}

The above methods read and write ALL the file content at once, in a single variable or buffer. For this reason they are not suggested when dealing with big files, which require a more memory efficient approach, for example reading a file line by line or by using a fixed size buffer.

{{% /warning %}}

## Read a file line by line

In order to read a text file line by line you can use the method `readLines()` provided by the file object, which returns the file content as a list of strings:

{{< highlight groovy >}}
    myFile = file('some/my_file.txt')
    allLines  = myFile.readLines()
    for( line : allLines ) {
        println line
    }
{{< /highlight >}}

This can also be written in a more idiomatic syntax:

{{< highlight groovy >}}
    file('some/my_file.txt')
        .readLines()
        .each { println it }
{{< /highlight >}}

{{% note %}}

The method `readLines()` reads all the file content at once and returns a list containing all the lines. For this reason, do not use it to read big files.

{{% /note %}}

To process a big file, use the method `eachLine`, which reads only a single line at a time into memory:

{{< highlight groovy >}}
    count = 0
    myFile.eachLine {  str ->
            println "line ${count++}: $str"
        }
{{< /highlight >}}

## Advanced file reading operations

The classes `Reader` and `InputStream` provide fine control for reading text and binary files, respectively.

The method `newReader` creates a [Reader](http://docs.oracle.com/javase/7/docs/api/java/io/Reader.html) object for the given file that allows you to read the content as single characters, lines or arrays of characters:

{{< highlight groovy >}}
    myReader = myFile.newReader()
    String line
    while( line = myReader.readLine() ) {
        println line
    }
    myReader.close()
{{< /highlight >}}

The method `withReader` works similarly, but automatically calls the `close` method for you when you have finished processing the file. So, the previous example can be written more simply as:

{{< highlight groovy >}}
    myFile.withReader {
        String line
        while( line = myReader.readLine() ) {
            println line
        }
    }
{{< /highlight >}}

The methods `newInputStream` and `withInputStream` work similarly. The main difference is that they create an [InputStream](http://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html) object useful for writing binary data.

Here are the most important methods for reading from files:

| Name            | Description                                                                                                                                     |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| getText         | Returns the file content as a string value                                                                                                      |
| getBytes        | Returns the file content as byte array                                                                                                          |
| readLines       | Reads the file line by line and returns the content as a list of strings                                                                        |
| eachLine        | Iterates over the file line by line, applying the specified `closure <script-closure>`                                                          |
| eachByte        | Iterates over the file byte by byte, applying the specified `closure <script-closure>`                                                          |
| withReader      | Opens a file for reading and lets you access it with a [Reader](http://docs.oracle.com/javase/7/docs/api/java/io/Reader.html) object            |
| withInputStream | Opens a file for reading and lets you access it with an [InputStream](http://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html) object |
| newReader       | Returns a [Reader](http://docs.oracle.com/javase/7/docs/api/java/io/Reader.html) object to read a text file                                     |
| newInputStream  | Returns an [InputStream](http://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html) object to read a binary file                        |

Read the Java documentation for [Reader](http://docs.oracle.com/javase/7/docs/api/java/io/Reader.html) and [InputStream](http://docs.oracle.com/javase/7/docs/api/java/io/InputStream.html) classes to learn more about methods available for reading data from files.

## Advanced file writing operations

The `Writer` and `OutputStream` classes provide fine control for writing text and binary files, respectively, including low-level operations for single characters or bytes, and support for big files.

For example, given two file objects `sourceFile` and `targetFile`, the following code copies the first file's content into the second file, replacing all `U` characters with `X`:

{{< highlight groovy >}}
    sourceFile.withReader { source ->
        targetFile.withWriter { target ->
            String line
            while( line=source.readLine() ) {
                target << line.replaceAll('U','X')
            }
        }
    }
{{< /highlight >}}

Here are the most important methods for writing to files:

| Name             | Description                                                                                                                                             |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| setText          | Writes a string value to a file                                                                                                                         |
| setBytes         | Writes a byte array to a file                                                                                                                           |
| write            | Writes a string to a file, replacing any existing content                                                                                               |
| append           | Appends a string value to a file without replacing existing content                                                                                     |
| newWriter        | Creates a [Writer](http://docs.oracle.com/javase/7/docs/api/java/io/Writer.html) object that allows you to save text data to a file                     |
| newPrintWriter   | Creates a [PrintWriter](http://docs.oracle.com/javase/7/docs/api/java/io/PrintWriter.html) object that allows you to write formatted text to a file     |
| newOutputStream  | Creates an [OutputStream](http://docs.oracle.com/javase/7/docs/api/java/io/OutputStream.html) object that allows you to write binary data to a file     |
| withWriter       | Applies the specified closure to a [Writer](http://docs.oracle.com/javase/7/docs/api/java/io/Writer.html) object, closing it when finished              |
| withPrintWriter  | Applies the specified closure to a [PrintWriter](http://docs.oracle.com/javase/7/docs/api/java/io/PrintWriter.html) object, closing it when finished    |
| withOutputStream | Applies the specified closure to an [OutputStream](http://docs.oracle.com/javase/7/docs/api/java/io/OutputStream.html) object, closing it when finished |

Read the Java documentation for the [Writer](http://docs.oracle.com/javase/7/docs/api/java/io/Writer.html), [PrintWriter](http://docs.oracle.com/javase/7/docs/api/java/io/PrintWriter.html) and [OutputStream](http://docs.oracle.com/javase/7/docs/api/java/io/OutputStream.html) classes to learn more about methods available for writing data to files.

## List directory content

Let's assume that you need to walk through a directory of your choice. You can define the `myDir` variable that points to it:

{{< highlight groovy >}}
    myDir = file('any/path')
{{< /highlight >}}

The simplest way to get a directory list is by using the methods `list` or `listFiles`, which return a collection of first-level elements (file s and directories) of a directory:

{{< highlight groovy >}}
    allFiles = myDir.list()
    for( def file : allFiles ) {
        println file
    }
{{< /highlight >}}

{{% note %}}

The only difference between `list` and `listFiles` is that the former returns a list of strings, and the latter a list of file objects that allow you to access file metadata, e.g. size, last modified time, etc.

{{% /note %}}

The `eachFile` method allows you to iterate through the first-level elements only (just like `listFiles`). As with other _each-_ methods, `eachFiles` takes a closure as a parameter:

{{< highlight groovy >}}
    myDir.eachFile { item ->
        if( item.isFile() ) {
            println "${item.getName()} - size: ${item.size()}"
        }
        else if( item.isDirectory() ) {
            println "${item.getName()} - DIR"
        }
    }
{{< /highlight >}}

Several variants of the above method are available. See the table below for a complete list.

| Name            | Description                                                                                                                                                                                                  |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eachFile        | Iterates through first-level elements (files and directories). [Read more](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/io/File.html#eachFile(groovy.io.FileType,%20groovy.lang.Closure))         |
| eachDir         | Iterates through first-level directories only. [Read more](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/io/File.html#eachDir(groovy.lang.Closure))                                                |
| eachFileMatch   | Iterates through files and dirs whose names match the given filter. [Read more](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/io/File.html#eachFileMatch(java.lang.Object,%20groovy.lang.Closure)) |
| eachDirMatch    | Iterates through directories whose names match the given filter. [Read more](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/io/File.html#eachDirMatch(java.lang.Object,%20groovy.lang.Closure))     |
| eachFileRecurse | Iterates through directory elements depth-first. [Read more](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/io/File.html#eachFileRecurse(groovy.lang.Closure))                                      |
| eachDirRecurse  | Iterates through directories depth-first (regular files are ignored). [Read more](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/io/File.html#eachDirRecurse(groovy.lang.Closure))                  |

See also: Channel `channel-path` method.

## Create directories

Given a file variable representing a nonexistent directory, like the following:

{{< highlight groovy >}}
    myDir = file('any/path')
{{< /highlight >}}

the method `mkdir` creates a directory at the given path, returning `true` if the directory is created successfully, and `false` otherwise:

{{< highlight groovy >}}
    result = myDir.mkdir()
    println result ? "OK" : "Cannot create directory: $myDir"
{{< /highlight >}}

{{% note %}}

If the parent directories do not exist, the above method will fail and return `false`.

{{% /note %}}

The method `mkdirs` creates the directory named by the file object, including any nonexistent parent directories:

{{< highlight groovy >}}
    myDir.mkdirs()
{{< /highlight >}}

## Create links

Given a file, the method `mklink` creates a *file system link* for that file using the path specified as a parameter:

{{< highlight groovy >}}
    myFile = file('/some/path/file.txt')
    myFile.mklink('/user/name/link-to-file.txt')
{{< /highlight >}}

Table of optional parameters:

| Name      | Description                                                                                                                                                                                                             |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| hard      | When `true` creates a *hard* link, otherwise creates a *soft* (aka *symbolic*) link. (default: `false`)                                                                                                                 |
| overwrite | When `true` overwrites any existing file with the same name, otherwise throws a [FileAlreadyExistsException](http://docs.oracle.com/javase/8/docs/api/java/nio/file/FileAlreadyExistsException.html) (default: `false`) |

## Copy files

The method `copyTo` copies a file into a new file or into a directory, or copy a directory to a new directory:

{{< highlight groovy >}}
    myFile.copyTo('new_name.txt')
{{< /highlight >}}

{{% note %}}

If the target file already exists, it will be replaced by the new one. Note also that, if the target is a directory, the source file will be copied into that directory, maintaining the file's original name.

{{% /note %}}

When the source file is a directory, all its content is copied to the target directory:

{{< highlight groovy >}}
    myDir = file('/some/path')
    myDir.copyTo('/some/new/path')
{{< /highlight >}}

If the target path does not exist, it will be created automatically.

{{% tip %}}

The `copyTo` method mimics the semantics of the Linux command `cp -r <source> <target>`, with the following caveat: While Linux tools often treat paths ending with a slash (e.g. `/some/path/name/`) as directories, and those not (e.g. `/some/path/name`) as regular files, Nextflow (due to its use of the Java files API) views both these paths as the same file system object. If the path exists, it is handled according to its actual type (i.e. as a regular file or as a directory). If the path does not exist, it is treated as a regular file, with any missing parent directories created automatically.

{{% /tip %}}

## Move files

You can move a file by using the method `moveTo`:

{{< highlight groovy >}}
    myFile = file('/some/path/file.txt')
    myFile.moveTo('/another/path/new_file.txt')
{{< /highlight >}}

{{% note %}}

When a file with the same name as the target already exists, it will be replaced by the source. Note also that, when the target is a directory, the file will be moved to (or within) that directory, maintaining the file's original name.

{{% /note %}}

When the source is a directory, all the directory content is moved to the target directory:

{{< highlight groovy >}}
    myDir = file('/any/dir_a')
    myDir.moveTo('/any/dir_b')
{{< /highlight >}}

Please note that the result of the above example depends on the existence of the target directory. If the target directory exists, the source is moved into the target directory, resulting in the path:

{{< highlight groovy >}}
    /any/dir_b/dir_a
{{< /highlight >}}

If the target directory does not exist, the source is just renamed to the target name, resulting in the path:

{{< highlight groovy >}}
    /any/dir_b
{{< /highlight >}}

{{% tip %}}

The `moveTo` method mimics the semantics of the Linux command `mv <source> <target>`, with the same caveat as that given for `copyTo`, above.

{{% /tip %}}

## Rename files

You can rename a file or directory by simply using the `renameTo` file method:

{{< highlight groovy >}}
    myFile = file('my_file.txt')
    myFile.renameTo('new_file_name.txt')
{{< /highlight >}}

## Delete files

The file method `delete` deletes the file or directory at the given path, returning `true` if the operation succeeds, and `false` otherwise:

{{< highlight groovy >}}
    myFile = file('some/file.txt')
    result = myFile.delete()
    println result ? "OK" : "Can delete: $myFile"
{{< /highlight >}}

{{% note %}}

This method deletes a directory ONLY if it does not contain any files or sub-directories. To delete a directory and ALL its content (i.e. removing all the files and sub-directories it may contain), use the method `deleteDir`.

{{% /note %}}

## Check file attributes

The following methods can be used on a file variable created by using the `file` method:

| Name          | Description                                                                           |
|---------------|---------------------------------------------------------------------------------------|
| getName       | Gets the file name e.g. `/some/path/file.txt` -\> `file.txt`                          |
| getBaseName   | Gets the file name without its extension e.g. `/some/path/file.tar.gz` -\> `file.tar` |
| getSimpleName | Gets the file name without any extension e.g. `/some/path/file.tar.gz` -\> `file`     |
| getExtension  | Gets the file extension e.g. `/some/path/file.txt` -\> `txt`                          |
| getParent     | Gets the file parent path e.g. `/some/path/file.txt` -\> `/some/path`                 |
| size          | Gets the file size in bytes                                                           |
| exists        | Returns `true` if the file exists, or `false` otherwise                               |
| isEmpty       | Returns `true` if the file is zero length or does not exist, `false` otherwise        |
| isFile        | Returns `true` if it is a regular file e.g. not a directory                           |
| isDirectory   | Returns `true` if the file is a directory                                             |
| isHidden      | Returns `true` if the file is hidden                                                  |
| lastModified  | Returns the file last modified timestamp i.e. a long as Linux epoch time              |

For example, the following line prints a file name and size:

{{< highlight groovy >}}
    println "File ${myFile.getName() size: ${myFile.size()}"
{{< /highlight >}}

{{% tip %}}

The invocation of any method name starting with the `get` prefix can be shortcut omitting the _get_ prefix and ending `()` parentheses. Therefore writing `myFile.getName()` is exactly the same of `myFile.name` and `myFile.getBaseName()` is the same of `myFile.baseName` and so on.

{{% /tip %}}

## Get and modify file permissions

Given a file variable representing a file (or directory), the method `getPermissions` returns a 9-character string representing the file's permissions using the [Linux symbolic notation](http://en.wikipedia.org/wiki/File_system_permissions#Symbolic_notation) e.g. `rw-rw-r--`:

{{< highlight groovy >}}
    permissions = myFile.getPermissions()
{{< /highlight >}}

Similarly, the method `setPermissions` sets the file's permissions using the same notation:

{{< highlight groovy >}}
    myFile.setPermissions('rwxr-xr-x')
{{< /highlight >}}

A second version of the `setPermissions` method sets a file's permissions given three digits representing, respectively, the _owner_, _group_ and _other_ permissions:

{{< highlight groovy >}}
    myFile.setPermissions(7,5,5)
{{< /highlight >}}

Learn more about [File permissions numeric notation](http://en.wikipedia.org/wiki/File_system_permissions#Numeric_notation).

## HTTP/FTP files

Nextflow provides transparent integration of HTTP/S and FTP protocols for handling remote resources as local file system objects. Simply specify the resource URL as the argument of the _file_ object:

{{< highlight groovy >}}
    pdb = file('http://files.rcsb.org/header/5FID.pdb')
{{< /highlight >}}

Then, you can access it as a local file as described in the previous sections:

{{< highlight groovy >}}
    println pdb.text
{{< /highlight >}}

The above one-liner prints the content of the remote PDB file. Previous sections provide code examples showing how to stream or copy the content of files.

{{% note %}}

Write and list operations are not supported for HTTP/S and FTP files.

{{% /note %}}

## Counting records

### countLines

The `countLines` methods counts the lines in a text files. :

{{< highlight groovy >}}
    def sample = file('/data/sample.txt')
    println sample.countLines()
{{< /highlight >}}

Files whose name ends with the `.gz` suffix are expected to be GZIP compressed and automatically uncompressed.

### countFasta

The `countFasta` method counts the number of records in [FASTA](https://en.wikipedia.org/wiki/FASTA_format) formatted file. :

{{< highlight groovy >}}
    def sample = file('/data/sample.fasta')
    println sample.countFasta()
{{< /highlight >}}

Files whose name ends with the `.gz` suffix are expected to be GZIP compressed and automatically uncompressed.

### countFastq

The `countFastq` method counts the number of records in a [FASTQ](https://en.wikipedia.org/wiki/FASTQ_format) formatted file. :

{{< highlight groovy >}}
    def sample = file('/data/sample.fastq')
    println sample.countFastq()
{{< /highlight >}}

Files whose name ends with the `.gz` suffix are expected to be GZIP compressed and automatically uncompressed.
