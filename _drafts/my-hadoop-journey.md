---
layout: post
title: My Hadoop journey
---

Hadoop is a big subject. I am going to approach it this way:

1. Do some hands-on work
2. Find out what I just did

##Prerequisites

####My environment:

- Ubuntu 14.10
- Java 7
- SSH (required packages: openssh-client and openssh-server)
- Hadoop 2.6.0 (from [Hadoop Download Mirrors](http://www.apache.org/dyn/closer.cgi/hadoop){:target="_blank"})

####Environment variables

- JAVA_HOME (set to the root of your Java installation)
- PATH (append /path/to/hadoop-installation/bin to the existing PATH)

####SSH setup

You need to be able to ssh to localhost without a password. Try this command:

~~~
ssh localhost
~~~

If you are prompted for a password, do the following:

~~~
ssh-keygen -t rsa -P ''
cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
~~~

hadoop jar hadoop-2.6.0/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar <command> <input dir> <output dir>
#output dir must not exist

hadoop jar hadoop-2.6.0/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar wordcount work_hadoop/ output
