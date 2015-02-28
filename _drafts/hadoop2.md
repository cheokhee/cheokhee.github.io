---
layout: post
title: Learning Hadoop, part &#35;2
---

 single-node in a pseudo-distributed mode

config changes:

etc/hadoop/hadoop-env.sh

export JAVA_HOME=

etc/hadoop/core-site.xml:
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>

etc/hadoop/hdfs-site.xml:
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>

run MapReduec locally:
1. format filesytem: (one-time)
hdfs namenode -format
2. Start NameNode daemon and DataNode daemon
start-dfs.sh
-->
started namenode,datanode,secondarynamenode,
/Data/jdk1.7.0/bin/java -Dproc_namenode org.apache.hadoop.hdfs.server.namenode.NameNode
/Data/jdk1.7.0/bin/java -Dproc_datanode org.apache.hadoop.hdfs.server.datanode.DataNode
/Data/jdk1.7.0/bin/java -Dproc_secondarynamenode org.apache.hadoop.hdfs.server.namenode.SecondaryNameNode

port 50020, 50010, 50075: DataNode
9000, 50070: NameNode
50090: SecondaryNameNode

4245 Jps
3976 SecondaryNameNode
3785 DataNode
3628 NameNode


web interface to NameNode
http://localhost:50070/
web interface to DataNode
http://localhost:50075/
web interface to SecondaryNameNode
http://localhost:50090/

one-time only:
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/<username>

copy files into distributed filesystem
hdfs dfs -put $HADOOP_PREFIX/etc/hadoop input

hdfs dfs -ls input

~~~
hadoop jar \
$HADOOP_PREFIX/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar \
wordcount input output
~~~

hdfs dfs -ls output

hdfs dfs -cat output/*

stop-dfs.sh


http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-common/SingleCluster.html

$HADOOP_PREFIX/logs


extra: hdfs dfs -rm -r output # remove directory


YARN:

create etc/hadoop/mapred-site.xml from etc/hadoop/mapred-site.xml.template
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>

etc/hadoop/yarn-site.xml :

<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>

assumption: filesystem has been formated, namenode and datanode daemons are running , user directory has been created:

start-yarn.sh
8342 ResourceManager
8476 NodeManager

ResourceManager listens on ports: 8088, 8030, 8031, 8032, 8033
NodeManager listens on: 13562, 57472, 8040, 8042

ResourceManager web interface: http://localhost:8088
NodeManager web interface http://localhost:8042
stop-yarn.sh
