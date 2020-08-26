# Spark SQL之DataFrame

         前面我们介绍过Spark对RDD进行封装得到的一个分布式的数据集，它与RDD最大的不同在于提供的更像是一个传统的数据库里面的表，他除了数据之外还能够知道更多的信息，比如说列名、列值和列的属性，这一点就和hive很类似了，而且他也能够支持一些复杂的数据格式。同时DataFrame可以从不同来源的数组构造，例如Hive表，结构化数据文件，外部数据库或现有RDD。

## **1、DataFrame的创建**

在Spark SQL中SparkSession是创建DataFrame和执行sql的入口，创建DataFrame有三种方式：通过Spark的数据源进行创建；从Hive Table进行查询返回；从一个存在的RDD进行转换。

### 1）从Spark数据源进行创建

spark支持创建数据源文件的格式主要有 **csv format jdbc json load option options orc parquet schema table text textFile。**在spark-shell中为我们提供了一个SparkSession对象叫做spark，我们先通过这个对象初步了解一下Spark SQL。通过spark.read. 查看spark支持创建数据源文件的格式

![](../.gitbook/assets/image%20%2857%29.png)

我们举例读取一个json文件，用DataFrame来表示：

![](../.gitbook/assets/image%20%2849%29.png)

通过spark.read.json从json文件中读取数据，并返回一个DataFrame类型的对象，通过这个对象可以查看文件中的数据。

### 2）从Hive Table进行查询返回

陆续补充。。。。。。

### 3）从RDD进行转换

陆续补充。。。。。。

## 2、SQL语法

SQL语法风格指的是我们查询数据的时候使用SQL语句来查询，这种风格的查询必须要有临时视图或者全局视图来辅助。下面举例来说明：

### 1）从json文件中读取数据

![](../.gitbook/assets/image%20%2847%29.png)

**返回一个DataFrame对象，其中包含json文件中结构化数据。接下来我们使用DataFrame对象中的方法来创建视图。**

### 2）创建一个全局临时视图

![](../.gitbook/assets/image%20%2858%29.png)

**通过createOrReplaceTempView\(\)方法为df对象创建一个临时的视图，为SQL查询提供。**

### 3）通过SQL语句实现查询全表

![](../.gitbook/assets/image%20%2848%29.png)

上面我们通过createOrReplaceTempView\(\)方法创建临时视图，通过spark环境对象提供的sql\(\)方法，对临时视图进行查询，这里之所以叫视图和数据库中的视图的概念类似，我们得到的是SQL查询的结果，即只能用于查询，无法对原数据进行增加和修改的操作。

{% hint style="info" %}
注意，对于普通临时视图是在Session范围内的，如果想要应用范围内有效，可以使用全局临时视图\(对应的方法是createOrReplaceGlobalTempView\(\)\)。此时如果访问全局临时视图需要全路径访问，如：global\_temp.user
{% endhint %}

![](../.gitbook/assets/image%20%2845%29.png)

我们先通过createOrReplaceGlobalTempView\(\)方法创建一个全局临时视图，在通过全路径访问视图。这样当我们创建一个新的Session连接的时候，依旧可以访问全局的临时视图。

![](../.gitbook/assets/image%20%2852%29.png)

通过全局临时视图，就可以跨越不同的Session进行访问。

## 3、DSL语法

         DataFrame提供了一个特定领域语言\(domain-specific language,DSL\)去管理结构化数据，使用DSL语法就不必去创建临时视图了。我们也可以在Java、python、Scala和R中使用DSL。下面举例来说明：

### 1）创建一个DataFrame

![](../.gitbook/assets/image%20%2847%29.png)

依旧是通过读取json文件来创建一个DataFrame的对象。

### 2）查看DataFrame的Schema信息——**printSchema** 

这里我们使用DataFrame对象的 **printSchema** 方法进行查询

![](../.gitbook/assets/image%20%2855%29.png)

### 3）查看“name”列的数据以及"age+1"的数据——select

这里我们使用DataFrame对象的 **select** 方法进行查询

![](../.gitbook/assets/image%20%2856%29.png)

**如果涉及到运算操作的时候，每列都必须使用$或者是引号的形式表达\(单引号+字段名\)，例如年龄+1的操作。同时类似于数据库，我们可以将age+1操作后的结果从新起一个别名，则在其中加入as + "别名"。**

![](../.gitbook/assets/image%20%2859%29.png)

### 4）查看年龄大于20的数据——filter

这里我们使用DataFrame对象的 **filter** 方法进行查询。

![](../.gitbook/assets/image%20%2851%29.png)

### 5）查看年龄分组的数据——groupBy

这里我们使用DataFrame对象的 **groupBy** 方法进行查询。

![](../.gitbook/assets/image%20%2854%29.png)



## 4、DataFrame与RDD的相互转换

在开始我们说过DataFrame的创建方式有三种，其中一种就是从现有的RDD进行转换，那么就看看如何将两者进行转换\(注意：在IDEA开发时，将DF和RDD进行互操作时，必须要引入  import spark.implicits.\_     在spark-shell中则无需引入\)

### 1）RDD转换成DataFrame

这里我们可以通过RDD对象中的 **toDF** 方法将RDD转换成DataFrame类型。

![](../.gitbook/assets/image%20%2861%29.png)

上例创建一个rdd对象，通过toDF方法，告知该值得列名为id，将RDD转化成DataFrame对象。

### 2）DataFrame转换成RDD







