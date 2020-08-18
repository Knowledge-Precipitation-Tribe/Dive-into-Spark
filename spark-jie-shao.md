# Spark初识

**什么是Spark**

Spark是为大规模数据处理而设计的快速通用的计算引擎，类似于Hadoop中的MapReduce模块，用于对大数据的处理。

**为什么学Spark**

Hadoop 已经成了大数据技术的事实标准，可能有人会问既然有了Hadoop，为什么还要有Spark的存在呢？

首先，我们要知道Spark和Hadoop不是一个东西，那两者是什么关系呢？别着急，我们先来了解一下

* _什么是Hadoop_

> Hadoop 是一种分析和处理大数据的软件平台，在大量计算机组成的集群中实现了对海量数据的分布式计算。 Hadoop 采用 MapReduce 分布式计算框架，根据 GFS 原理开发了 HDFS（分布式文件系统），并根据 BigTable 原理开发了HBase数据存储系统。
>
> Hadoop 的生态系统，主要由 HDFS、MapReduce， HBase， Zookeeper， Pig、 Hive 等核心组件构成，另外还包括 Sqoop、Flume 等框架，用来与其他企业系统融合。
>
> HDFS提供高可用的获取应用数据的分布式文件系统；MapReduce并行处理大数据集的编程模型；HBase是可扩展的分布式数据库，支持大表的结构化数据存储，建立在 HDFS 之上；Hive建立在 Hadoop 上的数据仓库基础构架，可以用来进行数据提取转化加载，这是一种可以存储、查询和分析存储在 Hadoop 中的大规模数据的机制。

![Hadoop&#x751F;&#x6001;&#x7CFB;&#x7EDF;](.gitbook/assets/image%20%281%29.png)

在大致了解什么是Hadoop之后，我们再来看看Spark和Hadoop的关系。在Hadoop中，其最关键的就是HDFS和MapReduce，其中MapReduce则是用来对大规模数据集合进行批处理操作，但是其本身存在的延迟过高，无法胜任实时、快速计算需求的问题，导致Spark的应运而生。

* Spark和MapReduce的区别

Spark继承了MapReduce分布式并行计算的优点，同时对其缺点进行了改进。相对于MapReduce有如下优点：

1. **高效性**：Spark 提供了内存计算，把中间结果放到内存中，带来了更高的迭代运算效率。Spark 减少了迭代过程中数据需要写入磁盘的需求，提高了处理效率。
2. **易用性**： Spark 提供的数据集操作类型更加丰富，从而可以支持更多类型的应用。
3. **通用性：**Spark可以用于批处理、交互式查询（Spark SQL）、实时流处理（Spark Streaming）、机器学习（Spark MLlib）和图计算（GraphX）。这些不同类型的处理都可以在同一个应用中无缝使用。
4. **兼容性：**Spark可以非常方便地与其他的开源产品进行融合。比如，Spark可以使用Hadoop的YARN和Apache Mesos作为它的资源管理和调度器等。

总的来说，Spark是一个基于内存的迭代计算框架，需要配合许多其他分布式工具才能进行很好的使用，因此Spark可以算是基于Hadoop的一个更高效的计算框架。

**Spark核心组件**

Spark的组成主要有：SparkCore、SparkSQL、SparkStreaming、MLlib、GraphX。其中SparkCore将分布式数据抽象为弹性分布式数据集（RDD），实现了应用任务调度、RPC、序化和压缩，并为运行在其上的上层组件提供API。SparkSQL是Spark来操作结构化数据的程序包，可以让我使用SQL语句的方式来查询数据，Spark支持 多种数据源，包含Hive表，parquest以及JSON等内容。SparkStreaming提供的实时数据进行流式计算的组件。MLlib提供常用机器学习算法的实现库。GraphX提供一个分布式图计算框架，能高效进行图计算。其中Spark还支持多种大数据处理模型、支持多种编程语言接口：Java、Scala、Python。

![Spark&#x6838;&#x5FC3;&#x7EC4;&#x4EF6;&#x56FE;](.gitbook/assets/image%20%283%29.png)

 











\*\*\*\*





