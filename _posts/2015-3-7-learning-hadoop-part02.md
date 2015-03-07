---
layout: post
title: Learning Hadoop, part &#35;2
---

##Pseudo-distributed mode (single node)

###Configuration changes:

####File #1: $HADOOP_PREFIX/etc/hadoop/hadoop-env.sh

Find the placeholder for JAVA_HOME and initialize it to the root of your Java installation

####File #2: $HADOOP_PREFIX/etc/hadoop/core-site.xml

~~~
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
~~~

The `fs.defaultFS` property is used to specify the NameNode URI.

####File #3: $HADOOP_PREFIX/etc/hadoop/hdfs-site.xml

~~~
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>

    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///var/tmp/hadoop/dfs/name</value>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///var/tmp/hadoop/dfs/data</value>
    </property>

</configuration>
~~~

- The `dfs.replication` property specifies the number of replication. Set it to 1 on a single-node setup.
- The `dfs.namenode.name.dir` property specifies where the NameNode should store its data. If unspecified, `/tmp` will be used.
- The `dfs.datanode.data.dir` property specifies where the DataNode should store its data. If unspecified, `/tmp` will be used.

####File #4: $HADOOP_PREFIX/etc/hadoop/mapred-site.xml

Create this file from $HADOOP_PREFIX/etc/hadoop/mapred-site.xml.template

~~~
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
~~~

Use YARN to execute MapReduce applications.

####File #5: $HADOOP_PREFIX/etc/hadoop/yarn-site.xml

~~~
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
~~~

###Execution

####Step #1: Format the filesytem

~~~
hdfs namenode -format
~~~

This is a one-time thing.

####Step #2: Start HDFS daemons

~~~
start-dfs.sh
~~~

This will start three processes: NameNode, SecondaryNameNode, DataNode

####Step #3: Start YARN daemons

~~~
start-yarn.sh
~~~

This will start two processes: ResourceManager and NodeManager

####Step #4: Create some user directories (one-time only)

~~~
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/<username>
~~~

As a sanity check, do `hdfs dfs -ls /user`

####Step #5: Copy some files into the distributed filesytem

~~~
hdfs dfs -put $HADOOP_PREFIX/etc/hadoop input
~~~

`input` is the name of the input directory. As a sanity check, do `hdfs dfs -ls input`

####Step #6: Execute a MapReduce example

~~~
hadoop jar \
$HADOOP_PREFIX/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar \
wordcount input output
~~~

####Step #7: Look at the output

~~~
hdfs dfs -cat output/*
~~~

####Step #8: Look at the web interfaces

- NameNode: `http://localhost:50070`
- DataNode: `http://localhost:50075`
- SecondaryNameNode: `http://localhost:50090`
- ResourceManager: `http://localhost:8088`
- NodeManager: `http://localhost:8042`

####Step #9: Stop the daemons

~~~
stop-dfs.sh
stop-yarn.sh
~~~

####Extra

Logs are in `$HADOOP_PREFIX/logs`
