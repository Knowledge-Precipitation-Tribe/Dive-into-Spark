# Spark SQL之DataFrame

         前面我们介绍过Spark对RDD进行封装得到的一个分布式的数据集，它与RDD最大的不同在于提供的更像是一个传统的数据库里面的表，他除了数据之外还能够知道更多的信息，比如说列名、列值和列的属性，这一点就和hive很类似了，而且他也能够支持一些复杂的数据格式。同时DataFrame可以从不同来源的数组构造，例如Hive表，结构化数据文件，外部数据库或现有RDD。

## **1、DataFrame的创建**

在Spark SQL中SparkSession是创建DataFrame和执行sql的入口，创建DataFrame有三种方式：通过Spark的数据源进行创建；从Hive Table进行查询返回；从一个存在的RDD进行转换。

### 1）从Spark数据源进行创建

spark支持创建数据源文件的格式主要有 **csv format jdbc json load option options orc parquet schema table text textFile。**在spark-shell中为我们提供了一个SparkSession对象叫做spark，我们先通过这个对象初步了解一下Spark SQL。通过spark.read. 查看spark支持创建数据源文件的格式

![](../.gitbook/assets/image%20%2847%29.png)

我们举例读取一个json文件，用DataFrame来表示：

![](../.gitbook/assets/image%20%2845%29.png)

通过spark.read.json从json文件中读取数据，并返回一个DataFrame类型的对象，通过这个对象可以查看文件中的数据。

### 2）从Hive Table进行查询返回

陆续补充。。。。。。

### 3）从RDD进行转换

陆续补充。。。。。。

## 2、SQL语法

SQL语法风格指的是我们查询数据的时候使用SQL语句来查询，这种风格的查询必须要有临时视图或者全局视图来辅助。下面举例来说明：

1）从json文件中读取数据













