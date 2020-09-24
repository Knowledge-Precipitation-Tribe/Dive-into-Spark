# Spark SQL

## Spark SQL 入口

在Spark core中，我们如果想要执行程序，需要构建一个上下文环境对象SparkContext，对于Spark SQL也同样如此，必须先创建一个SparkSession类，SparkSession是与集群交互的基础，也是SparkSQL的编码入口，并且所有的操作都要用到SparkSession提供的API。

在老版本中，SparkSQL提供两种入口，一是SQLContext对象，用于提供Spark自己进行SQL查询；二是HiveContext，用于连接Hive查询。在Spark2.0以后，统一使用SparkSession作为编码的入口。

### 1）SparkSession的创建

在2.0版本以后，SparkConf、SparkContext和SQLContext都已经被封装在SparkSession当中，实质上就是SQLContext和HiveContext的组合，所以在SQLContext和HiveContext上可用的API在SparkSession上同样是可以使用的。因此我们只需要创建SparkSession即可得到SparkSQL的入口，下面举例进行说明：

```text
val sparkConf = new SparkConf().setAppName("SparkSessionZipsExample").setMaster("local")
val spark = SparkSession
.builder()
.config(sparkConf)
.getOrCreate()
```

由于SparkSession  是一个私有类，因此无法创建对象，而Builder是一个构造器，通过 Builder, 可以添加各种配置。使用生成器的设计模式\(builder design pattern\)，如果我们没有创建SparkSession对象，则会实例化出一个新的SparkSession对象及其相关的上下文，即这里的getOrCreate\(\)方法。











