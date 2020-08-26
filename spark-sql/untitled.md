# Spark SQL

## Spark SQL 入口

在Spark core中，我们如果想要执行程序，需要构建一个上下文环境对象SparkContext，对于Spark SQL也同样如此，必须先创建一个SparkSession类，SparkSession是与集群交互的基础，也是SparkSQL的编码入口，并且所有的操作都要用到SparkSession提供的API。

在老版本中，SparkSQL提供两种入口，一是SQLContext对象，用于提供Spark自己进行SQL查询；二是HiveContext，用于连接Hive查询。在Spark2.0以后，统一使用SparkSession作为编码的入口。

