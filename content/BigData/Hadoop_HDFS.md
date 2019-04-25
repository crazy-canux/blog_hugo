Title: Hadoop HDFS
Date: 2016-04-11 22:57:37
Tags: Hadoop, HDFS



# HDFS

Hadoop Distributed File System: hadoop分布式文件系统

hadoop hdfs分为三部分:

NameNode -> JobTracker

secondary NameNode

DataNode -> TaskTracker

***

# hdfs commands

    hdfs [SHELL_OPTIONS] COMMAND [GENERIC_OPTIONS] [COMMAND_OPTIONS]

user commands:

    $ hdfs classpath

    $ hdfs dfs # 参考 hadoop fs命令
    ...

admin commands:

    $ hdfs balancer
    ...

debug commands:

    $ hdfs verify

    $ hdfs recoverLease

***


