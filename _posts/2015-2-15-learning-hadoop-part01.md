---
layout: post
title: Learning Hadoop, part &#35;1
---

Hadoop is a big subject. I am going to approach it one small bite at a time.

##Prerequisites

####My environment:

- Ubuntu 14.10
- Java 7
- SSH (required packages: openssh-client and openssh-server)
- Hadoop 2.6.0 (from [Hadoop Download Mirrors](http://www.apache.org/dyn/closer.cgi/hadoop){:target="_blank"})

####Environment variables

- PATH (append /path/to/hadoop-installation/bin and /path/to/hadoop-installation/sbin to the existing PATH)
- HADOOP_PREFIX (set to the root of my Hadoop installation)

####SSH setup

Hadoop requires that I be able to ssh to localhost without a password. Here's the command to check:

~~~
ssh localhost
~~~

Here's what needs to be done:

~~~
ssh-keygen -t rsa -P ''
cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys
~~~

##Standalone mode

This is the mode where Hadoop is run as a single Java process.

###Example #1

Count all the words in the input files.

####Step #1. Prepare input files

Create a new directory and populate it with text files (I just copy $HADOOP_PREFIX/etc/hadoop/*.xml to this directory)

####Step #2. Run this command

~~~
hadoop jar \
$HADOOP_PREFIX/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar \
wordcount <input_dir> <output_dir>
~~~

`<input_dir>` is the directory created in step #1. `<output_dir>` must not already exist.

####Step #3. Look at the files in the output directory

###Example #2

Solve a sudoku puzzle

####Step #1. Create a puzzle file

An example can be found in the Hadoop source package
(hadoop-2.6.0-src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta).
Here's the contents of the file:

~~~
8 5 ? 3 9 ? ? ? ?
? ? 2 ? ? ? ? ? ?
? ? 6 ? 1 ? ? ? 2
? ? 4 ? ? 3 ? 5 9
? ? 8 9 ? 1 4 ? ?
3 2 ? 4 ? ? 8 ? ?
9 ? ? ? 8 ? 5 ? ?
? ? ? ? ? ? 2 ? ?
? ? ? ? 4 5 ? 7 8
~~~

####Step #2. Run this command

~~~
hadoop jar \
$HADOOP_PREFIX/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar \
sudoku puzzle1.dta
~~~

###Example #3

Calculate &pi;. Run this command:

~~~
hadoop jar \
$HADOOP_PREFIX/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar \
pi 10 100
~~~
