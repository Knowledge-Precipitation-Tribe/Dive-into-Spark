# Spark Streaming

## 1、简介：

Spark  Streaming是用来进行实时计算的一种框架，其基础的计算模型还是基于内存的大数据实时计算模型RDD，只不过，在RDD之上，进行了一层封装叫做DStream\(类似于Spark SQL中的DataFrame\)。同时它还可以用于进行大规模、高吞吐量、容错的实时数据流的处理；支持从很多种数据源中读取数据，比如Kafka、Flume、Twitter、ZeroMQ、Kinesis或者TCP Socket，并且能够使用类似高阶函数的复杂算法来进行数据处理，比如map、reduce、join、window；处理后的数据可以被保存到文件系统、数据库、Dashboard等存储中。

![Spark Streaming &#x6D41;&#x7A0B;&#x56FE;](.gitbook/assets/image%20%2871%29.png)

## 2、工作原理：

Spark Streaming的工作原理主要是在接收实时输入数据流，然后将数据拆分成多个batch，再將每个batch交给Spark计算引擎进行处理，最终产生一个结果数据流， **一个batchInterval\(每个batch的间隔\)累加读取到的数据对应一个RDD的数据。**

![&#x5DE5;&#x4F5C;&#x539F;&#x7406;](.gitbook/assets/image%20%2870%29.png)

## **3、DStream：**

**DStream（Discretized Stream）**成为离散流**，**是Spark Streaming提供的一种高级抽象，代表了一个持续不断的数据流。它可以以通过输入数据流来创建，比如Kafka,Flume，也可以通过对其他DStream应用高阶函数来创建，例如map、reduce、join、window。

#### DStream的特点：

* **不可变、分布式：**由于DStream的内部，其实是一系列持续不断产生的RDD，RDD是Spark Core的核心抽象，即，不可变的，分布式的数据集。 **DStream中的每个RDD都包含了一个时间段内的数据；**如下图：

![](.gitbook/assets/image%20%2868%29.png)

* **DStream应用算子，底层会被翻译为对DStream中每个RDD的操作**。比如对一个DStream执行一个map操作，会产生一个新的DStream，其底层原理为，对输入DStream中的每个时间段的RDD，都应用一遍map操作，然后生成的RDD，即作为新的DStream中的那个时间段的一个RDD。如下图：

![](.gitbook/assets/image%20%2869%29.png)

## 4、DStream编程：

下面先写一个小例子看一下如何编写DStream。

* **启动netcat服务器**：首先我们通过使用netcat来监听数据流端口

```text
$> nc -lk 9999
```

* **配置依赖**：通过使用IDEA进行编写，首先需要在pom.xml中引入Spreak Stream依赖。

```text
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-streaming_2.11</artifactId>
    <version>2.1.0</version>
</dependency>

```

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*

