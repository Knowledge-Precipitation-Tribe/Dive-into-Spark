# RDDs 的操作

RDD是一个只可读、可分区、可并行计算的数据集合，因此如果想修改RDD只能通过RDD的转换操作。RDD提供了丰富的操作以支持常见的数据处理，而RDD的操作主要包括两类，一类叫做**`Transformations`**，另一类叫做**`Actions`** 

其中Transformation操作主要是指定RDD之间的依赖关系，通过接受RDD对象并返回RDD；Actions操作则是执行计算，并指定输出的形式，通过接受RDD对象返回输出结果\(非RDD\)。对于RDD的一系列操作，主要流程如下：

![Spark RDD &#x7684;&#x6267;&#x884C;&#x8FC7;&#x7A0B;](../.gitbook/assets/image%20%2828%29.png)

**1、初始化Spark**

 Spark 编程的第一步是需要创建一个 SparkContext 对象，该对象的主要功能是告诉Spark 如何访问集群。在构建SparkContext对象之前，我们需要通过构建一个 SparkConf对象，其包含应用程序的信息。

```text
val conf = new SparkConf().setAppName("ParaCollections").setMaster("local")
val sc  = new SparkContext(conf)
```





































