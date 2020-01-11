---
layout: post
title:  "Hadoop linux配置"
date:   2019-03-01
excerpt: "Hadoop一些安装笔记"
tag:
- 大数据
- 笔记
---
# Hadoop linux配置

## 简述

hadoop 部署模式有：

### Hadoop本地模式

所有模块都运行与一个JVM进程中，使用的本地文件系统，而不是HDFS，本地模式主要是用于本地开发过程中的运行调试用。

### Hadoop伪分布式模式

Hadoop 进程以分离的 Java 进程来运行，节点既作为 NameNode 也作为 DataNode，同时，读取的是 HDFS 中的文件。

##### 配置

Hadoop 的配置文件位于 /usr/local/hadoop/etc/hadoop/ 中，伪分布式需要修改2个配置文件 core-site.xml 和 hdfs-site.xml 。Hadoop的配置文件是 xml 格式，每个配置以声明 property 的 name 和 value 的方式来实现。

>**Hadoop 的运行方式是由配置文件决定的（运行 Hadoop 时会读取配置文件），因此如果需要从伪分布式模式切换回非分布式模式，需要删除 core-site.xml 中的配置项.()**

`core-site.xml`

```xml
<configuration>
        <property>
             <name>hadoop.tmp.dir</name>
             <value>file:/usr/local/hadoop/tmp</value>
             <description>Abase for other temporary directories.</description>
        </property>
        <property>
             <name>fs.defaultFS</name>
             <value>hdfs://localhost:9000</value>
        </property>
</configuration>
```

`hdfs-site.xml`

```xml
<configuration>
        <property>
             <name>dfs.replication</name>
             <value>1</value>
        </property>
        <property>
             <name>dfs.namenode.name.dir</name>
             <value>file:/usr/local/hadoop/tmp/dfs/name</value>
        </property>
        <property>
             <name>dfs.datanode.data.dir</name>
             <value>file:/usr/local/hadoop/tmp/dfs/data</value>
        </property>
</configuration>
```

配置完成后，执行 NameNode 的格式化：

```bash
$./bin/hdfs namenode -format
```

接着开启 NameNode 和 DataNode 守护进程：

```bash
$./sbin/start-dfs.sh
```

```bash
hadoop@parallels-vm:/usr/local/hadoop-3.2.0$ ./sbin/start-dfs.sh
Starting namenodes on [localhost]
Starting datanodes
Starting secondary namenodes [parallels-vm]
parallels-vm: Warning: Permanently added 'parallels-vm' (ECDSA) to the list of known hosts.
```

> 电脑卡爆

启动完成后，可以通过命令 jps 来判断是否成功启动，若成功启动则会列出如下进程: “NameNode”、”DataNode” 和 “SecondaryNameNode”（如果 SecondaryNameNode 没有启动，请运行 sbin/stop-dfs.sh 关闭进程，然后再次尝试启动尝试）。如果没有 NameNode 或 DataNode ，那就是配置不成功，请仔细检查之前步骤，或通过查看启动日志排查原因。
![](https://ws4.sinaimg.cn/large/006tKfTcly1g0nkkfi0ksj30uy063mxy.jpg)

成功启动后，可以访问 Web 界面 [http://localhost:50070](http://localhost:50070/) 查看 NameNode 和 Datanode 信息，还可以在线查看 HDFS 中的文件。

### 完全分布式

完全分布式模式才是生产环境采用的模式，Hadoop 运行在服务器集群上，生产环境一般都会做HA，以实现高可用。

### Hadoop HA

HA是指高可用，为了解决Hadoop单点故障问题，生产环境一般都做HA部署。这部分介绍了如何配置Hadoop2.x的高可用，并简单介绍了HA的工作原理。
