# RDDs 的操作

RDD是一个只可读、可分区、可并行计算的数据集合，因此如果想修改RDD只能通过RDD的转换操作。RDD提供了丰富的操作以支持常见的数据处理，而RDD的操作主要包括两类，一类叫做**`Transformations`**，另一类叫做**`Actions`** 

其中Transformation操作主要是指定RDD之间的依赖关系，通过接受RDD对象并返回RDD；Actions操作则是执行计算，并指定输出的形式，通过接受RDD对象返回输出结果\(非RDD\)。对于RDD的一系列操作，主要流程如下：

![Spark RDD &#x7684;&#x6267;&#x884C;&#x8FC7;&#x7A0B;](../.gitbook/assets/image%20%2829%29.png)

**1、初始化Spark**

 Spark 编程的第一步是需要创建一个 SparkContext 对象，该对象的主要功能是告诉Spark 如何访问集群。在构建SparkContext对象之前，我们需要通过构建一个 SparkConf对象，其包含应用程序的信息。

```text
val conf = new SparkConf().setAppName("ParaCollections").setMaster("local")
val sc  = new SparkContext(conf)
```

其中setAppName参数传递的是程序的名字，setMaster则是程序运行的主RUL。

SparkConf最常用的一些属性

* **set（key，value）** - 设置配置属性。
* **setMaster（value）** - 设置主URL。
* **setAppName（value）** - 设置应用程序名称。
* **get（key，defaultValue = None）** - 获取密钥的配置值。
* **setSparkHome（value）** - 在工作节点上设置Spark安装路径。

**2、创建RDD**

1）从集合创建RDD

通过在一个已有的集合\(Scala `Seq`\)上调用 SparkContext 的 `parallelize` 方法实现对RDD的创建。集合中的元素被复制到一个可并行操作的分布式数据集中。

```text
val ints = Array(1,2,3,4,5,6)
val rdd = sc.parallelize(ints,10)
```

其中`parallelize` 有一个重要的参数， 切片数\(_slices_\)，表示一个数据集切分的份数。正常情况下，Spark 会试着基于你的集群状况自动地设置切片的数目，当然你可以自己设置。

2）从外部存储创建RDD

RDD可以通过SparkContext 中的textFile访问本地数据 创建RDD对象，textFile方法需要春如一个文件的URL。

```text
val data1 = sc.textFile("src/main/scala/assets/1.txt")
```

 一旦创建完成，`data1` 就能做数据集操作，如map、reduce等。

3）从HDFS创建RDD

RDD也可以支持从HDFS文件创建RDD，常用的生产环境处理方式，主要可以针对HDFS上存储的大数据，进行离线批处理操作，其创建的方式依旧使用 textFile 

```text
# 从本地文件系统地址加载
rdd = sc.textFile("file:///path/to/news_sep.txt")
# 从分布式文件系统HDFS地址加载，下面三种方式等价
rdd = sc.textFile("hdfs://localhost:9000/path/to/news_sep.txt")
rdd = sc.textFile("/path/to/news_sep.txt")
rdd = sc.textFile("news_sep.txt")
```



{% hint style="info" %}
注意

1、如果是进行本地测试，本地对应路径上有一份文件即可；如果是在Spark集群上针对Linux本地文件，那么需要将文件拷贝到所有worker节点上（就是在spark-submit上使用—master指定了master节点，使用standlone模式进行运行，而textFile\(\)方法内仍然使用的是Linux本地文件，在这种情况下，是需要将文件拷贝到所有worker节点上的）； 

2、 Spark的textFile\(\)方法支持针对目录、压缩文件以及通配符进行RDD创建 

3、Spark默认会为hdfs文件的每一个block创建一个partition，但是也可以通过textFile\(\)的第二个参数手动设置分区数量，只能比block数量多，不能比block数量少
{% endhint %}

4）其他方法创建RDD

SparkContext的textFile\(\)除了可以针对上述几种普通的文件创建RDD之外，还有一些特例的方法来创建RDD：

* SparkContext的wholeTextFiles\(\)方法，可以针对一个目录中的大量小文件，返回由（fileName,fileContent）组成的pair，即pairRDD
*  SparkContext的sequenceFileK,V方法，可以针对SequenceFile创建RDD，K和V泛型类型就是SequenceFile的key和value的类型。
* SparkContext的hadoopRDD\(\)方法，对于Hadoop的自定义输入类型，可以创建RDD。该方法接收JobConf、InputFormatClass、Key和Value的Class。
* SparkContext的objectFile\(\)方法，可以针对之前调用的RDD的saveAsObjectFile\(\)创建的对象序列化的文件，反序列化文件中的数据，并创建一个RDD。

**3、RDD的转化操作**

官网API文档中给出了非常详细的介绍 [API文档](http://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)，下面简单介绍几个常用的RDD转换操作：

**1）Map\(\):**

Map\(func\)操作返回一个新的MapoedRDD，该数据集是通过将源RDD的每个元素传递给函数func形成的。

```text
object Map {
  def main(args: Array[String]) {
    val conf = new SparkConf().setMaster("local").setAppName("map")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(1 to 10)  //创建RDD
    val map = rdd.map(_*2)             //对RDD中的每个元素都乘于2
    map.foreach(x => print(x+" "))
    sc.stop()
  }
}
```

\(RDD依赖图：红色块表示一个RDD区，黑色块表示该分区集合，下同\)

![](../.gitbook/assets/image%20%2826%29.png)































