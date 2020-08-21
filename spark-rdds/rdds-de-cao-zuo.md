# RDDs 的操作

RDD是一个只可读、可分区、可并行计算的数据集合，因此如果想修改RDD只能通过RDD的转换操作。RDD提供了丰富的操作以支持常见的数据处理，而RDD的操作主要包括两类，一类叫做**`Transformations`**，另一类叫做**`Actions`** 

其中Transformation操作主要是指定RDD之间的依赖关系，通过接受RDD对象并返回RDD；Actions操作则是执行计算，并指定输出的形式，通过接受RDD对象返回输出结果\(非RDD\)。对于RDD的一系列操作，主要流程如下：

![Spark RDD &#x7684;&#x6267;&#x884C;&#x8FC7;&#x7A0B;](../.gitbook/assets/image%20%2839%29.png)

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

**3、基础RDD的转化操作**

官网API文档中给出了非常详细的介绍 [API文档](http://spark.apache.org/docs/latest/rdd-programming-guide.html#transformations)，下面简单介绍几个常用的RDD转换操作：

**1）Map\(func\):**

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

![](../.gitbook/assets/image%20%2835%29.png)

**2）flatMap\(func\)**

flatMap\(func\)操作和map类似，但每个元素输入项都可以被映射到0个或多个的输出项。

```text
//...省略sc
val rdd = sc.parallelize(1 to 5)
val fm = rdd.flatMap(x => (1 to x)).collect()
fm.foreach( x => print(x + " "))
```

这里注意floatMap的输出是扁平化的，和Map有所不同。

flatMap函数其输出如下：

```text
1 1 2 1 2 3 1 2 3 4 1 2 3 4 5
```

如果是map函数其输出如下：

```text
Range(1) Range(1, 2) Range(1, 2, 3) Range(1, 2, 3, 4) Range(1, 2, 3, 4, 5)
```

\(RDD依赖图\)

![](../.gitbook/assets/image%20%2833%29.png)

更多详细的例子 查看 Transformations详解 部分

{% page-ref page="transformations-xiang-jie.md" %}

**4、键值对RDD的转化操作**

**1）mapValues\(func\)**

**mapValues\(func\) 方法对\[K,V\]类型数据中的V进行map操作，例如下面对每个人的年龄加2**

```text
object MapValues {
  def main(args: Array[String]): Unit = {
    val conf = new SparkConf().setMaster("local").setAppName("MapValues")
    val src = new SparkContext(conf)
    val data = List(("张三",21),("李四",26),("王五",17))
    val rdd1 = src.parallelize(data)
    val mapValueRDD = rdd1.mapValues(_+2)
    mapValueRDD.foreach(println)
  }
}
```

 **\(RDD依赖图：红色块表示一个RDD区，黑色块表示该分区集合，下同\)**

![](../.gitbook/assets/image%20%2830%29.png)

**2）** **foldByKey** **\(zeroValue\)\(func\)**

 foldByKey\(zeroValue\)\(func\) ****函数是通过调用CombineByKey函数实现的，其中   **zeroVale：**对V进行初始化，通过CombineByKey的createCombiner实现的  V =&gt;  \(zeroValue,V\)，再通过func函数映射成新的值，即func\(zeroValue,V\)； **func**则是Value通过func函数按Key值进行合并。

```text
val data = List(("张三",21),("李四",26),("王五",17),("张三",17))
val rdd1 = src.parallelize(data)
val mapValueRDD = rdd1.foldByKey(1)(_+_)
mapValueRDD.foreach(println)
```

**3）** **reduceByKey\(func,numPartitions\)**

 reduceByKey\(func,numPartitions\) 函数按Key进行分组，使用给定的func函数聚合value值, numPartitions设置分区数，提高作业并行度。

```text
val conf = new SparkConf().setMaster("local").setAppName("ReduceByKey")
val src = new SparkContext(conf)
val data = List(("A",3),("A",2),("B",1),("B",3))
val rdd1 = src.parallelize(data)
val reduceByKeyRDD  = rdd1.reduceByKey(_+_)
reduceByKeyRDD.foreach(println)
```

 **\(RDD依赖图\)**

![](../.gitbook/assets/image%20%2832%29.png)

多详细的例子 查看 Transformations详解 部分 

{% page-ref page="transformations-xiang-jie.md" %}



**5、RDD的Actions操作**

 上面将的**Transformation**属于延迟计算，当一个RDD转换成另一个RDD时并没有立即进行转换，仅仅是记住       了数据集的逻辑操作，只有进行 **Ation（执行）**操作后才能触发Spark作业的运行，真正触发转换算子的计算。

官网API文档中给出了非常详细的介绍 [API文档](http://spark.apache.org/docs/latest/rdd-programming-guide.html#actions)，下面简单介绍几个常用的RDD的**Actions**操作：

**1）reduce\(func\)**

 **reduce\(func\):**通过函数func先聚集各分区的数据集，再聚集分区之间的数据，func接收两个参数，返回一个新值，新值再做为参数继续传递给函数func，直到最后一个元素**。**

```text
val rdd1 = src.parallelize(1 to 10 ,2)
val reduceRDD   = rdd1.reduce(_+_)
println("func +: "+reduceRDD)
```

**输出：55**

![](../.gitbook/assets/image%20%2838%29.png)

**2）lookup\(k\)**

**lookup\(k\)**函数作用于K-V类型的RDD上，返回指定K的所有V值

```text
val conf = new SparkConf().setAppName("LookUp").setMaster("local")
val sc = new SparkContext(conf)
val arr = List(("A", 1), ("B", 2), ("A", 2), ("B", 3))
val rdd = sc.parallelize(arr,2)

val lookupRDD = rdd.lookup("A")

println("lookup:")
lookupRDD.foreach(x=> print(x+" "))
```

**输出：**

```text
lookup:
1 2 
```

![](../.gitbook/assets/image%20%2840%29.png)

**3\) aggregate\(zeroValue:U\)**

**aggregate\(zeroValue:U\)\(seqOp:\(U,T\) =&gt; U,comOp\(U,U\) =&gt; U\):**seqOp函数将每个分区的数据聚合成类型为U的值，comOp函数将各分区的U类型数据聚合起来得到类型为U的值

```text
def main(args: Array[String]) {
    val conf = new SparkConf().setMaster("local").setAppName("Fold")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(List(1,2,3,4),2)
    val aggregateRDD = rdd.aggregate(2)(_+_,_ * _)
    println(aggregateRDD)
    sc.stop
  }
```

**输出：**

```text
90
```

![](../.gitbook/assets/image%20%2827%29.png)



**4\)  fold\(zeroValue:T\)**

**fold\(zeroValue:T\)\(op:\(T,T\) =&gt; T\):**通过op函数聚合各分区中的元素及合并各分区的元素，op函数需要两个参数，在开始时第一个传入的参数为zeroValue,T为RDD数据集的数据类型，，其作用相当于SeqOp和comOp函数都相同的aggregate函数。

```text
def main(args: Array[String]) {
    val conf = new SparkConf().setMaster("local").setAppName("Fold")
    val sc = new SparkContext(conf)
    val rdd = sc.parallelize(Array(("a", 1), ("b", 2), ("a", 2), ("c", 5), ("a", 3)), 2)
    val foldRDD = rdd.fold(("d", 0))((val1, val2) => { if (val1._2 >= val2._2) val1 else val2
    })
    println(foldRDD)
  }
```

**输出：**

```text
c,5
```

![](../.gitbook/assets/image%20%2831%29.png)

**其过程如下：** 

1.开始时将\(“d”,0\)作为op函数的第一个参数传入，将Array中和第一个元素\("a",1\)作为op函数的第二个参数传入，并比较value的值，返回value值较大的元素。

2.将上一步返回的元素又作为op函数的第一个参数传入，Array的下一个元素作为op函数的第二个参数传入，比较大小。

3.重复第2步骤，每个分区的数据集都会经过以上三步后汇聚后再重复以上三步得出最大值的那个元素。



还有一些是比较简单的Actions 操作，在下面表格中：

| **Actions 名称** | 功能 |
| :---: | :---: |
|  **collect\(\)** |  **以数据的形式返回数据集中的所有元素给Driver程序，为防止Driver程序内存溢出，一般要控制返回的数据集大小** |
|  **count\(\)** |  **返回数据集元素个数** |
|  **first\(\)** | **返回数据集的第一个元素** |
| **take\(n\)** |  **以数组的形式返回数据集上的前n个元素** |
|  **top\(n\)** | **按默认或者指定的排序规则返回前n个元素，默认按降序输出** |
|  **takeOrdered\(n,\[ordering\]\)** | **按自然顺序或者指定的排序规则返回前n个元素** |
|  **countByKey\(\)** | **作用于K-V类型的RDD上，统计每个key的个数，返回\(K,K的个数\)** |
|  **collectAsMap\(\)** | **作用于K-V类型的RDD上，作用与collect不同的是collectAsMap函数不包含重复的key，对于重复的key。后面的元素覆盖前面的元素** |
|  **saveAsFile\(path:String\)** | **将最终的结果数据保存到指定的HDFS目录中** |
|  **saveAsSequenceFile\(path:String\)** | **将最终的结果数据以sequence的格式保存到指定的HDFS目录中** |



























