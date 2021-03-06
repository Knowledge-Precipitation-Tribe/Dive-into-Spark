# RDDs 的函数

下面主要是讲解RDDs的所有函数操作及相应的例子。

### **1、RDD的基本转换**

**1）Map\(func\)**函数返回一个新的MappedRDD，该数据集是通过将源RDD的每个元素传递给函数func形成的。

**2）flatMap\(func\)** 函数和Map类似，但是其可以将每个输入项映射到0或多个输出项，最终的结果是"扁平化"输出。

这两个函数操作可以在 **RDDs的操作** 例子。

**3）sample\(withReplacement,fraction,seed\)**函数进行随机采样，参数一：是否进行有放回采样；参数二：采样的个数所占整体样本的比例；参数三：随机种子，默认值是一个0~Long.maxvalue之间的整数。

```text
val conf = new SparkConf().setMaster("local").setAppName("Sample")
val sc = new SparkContext(conf)
val rdd = sc.parallelize(1 to 10)
val samle1 = rdd.sample(true,0.8,0)
samle1.foreach(x => print(x+" "))
sc.stop()
```

**输出：**

```text
2 4 5 6 8 8 8 9 10 
```

**4）union\(ortherDataset\)**函数将两个RDD中的数据集进行合并，最终返回两个RDD的并集，若RDD中存在相同的元素也不会去重。

```text
val conf = new SparkConf().setMaster("local").setAppName("union")
val sc = new SparkContext(conf)
val rdd1 = sc.parallelize(1 to 4)
val rdd2 = sc.parallelize(4 to 8)
val unionRDD  = rdd1.union(rdd2)
unionRDD.foreach(x => print(x + " "))
sc.stop()
```

**输出:**

```text
1 2 3 4 4 5 6 7 8
```

**5）intersection\(otherDataset\)**返回两个RDD的交集

```text
val rdd1 = sc.parallelize(1 to 4)
val rdd2 = sc.parallelize(4 to 8)
val intersectionRDD  = rdd1.intersection(rdd2)
```

**输出：**

```text
4
```

**6）distinct\(\[numTasks\]\)**对RDD中的元素进行去重

```text
val rdd1 = sc.parallelize(List(1,2,3,1,3,2,4,6,4,6,8,6,5,3,1,2,4,7))
val distinctRDD  = rdd1.distinct()
distinctRDD.foreach(x => print(x + " "))
```

**输出：**

```text
4 1 6 3 7 8 5 2 
```

**7）cartesian\(otherDataset\)**对两个RDD中的所有元素进行笛卡尔积操作

```text
val rdd1 = sc.parallelize(1 to 3)
val rdd2 = sc.parallelize(4 to 6)
val cartesianRDD  = rdd1.cartesian(rdd2)
cartesianRDD.foreach(x => print(x + " "))
```

**输出：**

```text
(1,4) (1,5) (1,6) (2,4) (2,5) (2,6) (3,4) (3,5) (3,6) 
```

**RDD依赖图：**

![](../.gitbook/assets/image%20%2842%29.png)

**8）10.coalesce\(numPartitions，shuffle\)**将分区数减少到numPartitions个分区的重新分区，参数一：分区数；参数二：是否进行打乱。怎么理解参数二呢？

当RDD有N个分区，需要重新划分成M个分区时：

* N&gt;M：如果N，M之间相差的不多时，那么就可以将N个分区中的若干个分区合并成一个新的分区，最终合并为M个分区，之间存在窄依赖关系；如果N，M之间相差悬殊时，若M为1的时候，为了使coalesce之前的操作有更好的并行度，可以将shuffle设置为true。
* N&lt;M：如果shuffle为false，不会进行分区，分区数依旧是N，之间为窄依赖；如果shuffle为true，重分区前后相当于宽依赖\(一般情况下N个分区有数据分布不均匀的状况，利用HashPartitioner函数将数据重新分区为M个\)

N&gt;M的情况：

```text
val rdd = sc.parallelize(1 to 16 ,4)
println("重新分区前的分区个数"+rdd.partitions.size)
val coalesceRDD = rdd.coalesce(3,false)
println("重新分区后的分区个数:"+coalesceRDD.partitions.size)
```

**输出：**

```text
重新分区前的分区个数4 重新分区后的分区个数:3
```

N&lt;M的情况：

```text
val rdd = sc.parallelize(1 to 16 ,4)
println("重新分区前的分区个数"+rdd.partitions.size)
val coalesceRDD = rdd.coalesce(7,true)
println("重新分区后的分区个数:"+coalesceRDD.partitions.size)
```

**输出:**

```text
重新分区前的分区个数4 重新分区后的分区个数:7
```

如果：val = coalesceRDD = rdd.coalesce\(7,false\)  输出为：

```text
重新分区前的分区个数4 重新分区后的分区个数:4
```

**9）** **repartition\(numPartition\)**是函数coalesce\(numPartition,true\)的实现，效果和coalesce\(numPartition,true\)的例子一样。

**10）** **glom\(\)**将RDD每个分区中的类型为T的元素转化为数组Array\[T\]

```text
val rdd = sc.parallelize(1 to 6,2)
val glomRDD = rdd.glom()
glomRDD.foreach(x => println(x.getClass.getSimpleName))
```

**输出：**

```text
int[]
int[]
```

RDD依赖关系图

![](../.gitbook/assets/image%20%2841%29.png)



 **11）randomSplit\(weight:Array\[Double\],seed\)**根据weight权重值将一个RDD划分成多个RDD,权重越高划分得到的元素较多的几率就越大。

```text
val rdd = sc.parallelize(1 to 10)
val randomSplitRDD = rdd.randomSplit(Array(1.0,3.0,6.0))
randomSplitRDD(0).foreach(x => print(x +" "))
randomSplitRDD(1).foreach(x => print(x +" "))
randomSplitRDD(2).foreach(x => print(x +" "))
```

**输出:**

```text
8 10 
2 6 9
1 3 4 5 7
```

### **2、键值RDD的转换函数**

**1）** **mapValus\(fun\):**对pairRDD中的每个值调用map而不改变键。之间是窄依赖关系

```text
val list = List(("zhangsan", 22), ("lisi", 20), ("wangwu", 23))
val rdd = sc.parallelize(list)
val mapValuesRDD = rdd.mapValues(_ + 2)
mapValuesRDD.foreach(println)
sc.stop()
```

**输出：**

```text
(zhangsan,24)
(lisi,22)
(wangwu,25)
```

**2）flatMapValues\(\)** 对pairRDD中的每个值调用flatMap而不改变键，与mapValues\(\)的区别在于floatMapValues可以将值映射成多个值\(多个值成列表型 Seq\)，而mapValues只能一对一映射。

```text
val rdd = sc.parallelize(List(("zhangsan", 22), ("lisi", 20), ("wangwu", 23)))
val flatMapValues = rdd.flatMapValues(x=> Seq(x,"male"))
flatMapValues.foreach(println)
```

**输出：**

```text
(mobin,22)
(mobin,male)
(kpop,20)
(kpop,male)
(lufei,23)
(lufei,male)
```

RDD依赖图：

![](../.gitbook/assets/image%20%2844%29.png)

**3）comineByKey**

**4）foldByKey\(zeroValue\)\(func\)**

**5）reduceByKey\(func,numPartitions\)** 对于每个分区中，按照Key值进行分组。参数一指的是聚合value值得函数；参数二指的是设置分区数，提高作业并行度。

```text
val data = List(("A", 3), ("A", 2), ("B", 1), ("B", 3))
val rdd1 = src.parallelize(data,2)
val reduceByKeyRDD = rdd1.reduceByKey(_ * _,2)
reduceByKeyRDD.foreach(println)
println(reduceByKeyRDD.partitions.size)
```

**输出：**

```text
(B,3)
(A,6)
2
```

**RDD**依赖图

![](../.gitbook/assets/image%20%2843%29.png)

**6）groupByKey\(numPartitions\)** 按Key进行分组，返回\[K,Iterable\[V\]\]，numPartitions设置分区数，提高作业并行度

```text
val data = List(("A",1),("B",2),("A",2),("B",3))
val rdd1 = src.parallelize(data,2)
val groupByKeyRDD = rdd1.groupByKey(2)
groupByKeyRDD.foreach(println)
```

**输出：**

```text
(B,CompactBuffer(2, 3))
(A,CompactBuffer(1, 2))
```

**7）sortByKey\(accending，numPartitions\)** 返回以Key排序的（K,V）键值对组成的RDD，accending为true时表示升序，为false时表示降序，numPartitions设置分区数，提高作业并行度。

```text
val data = List(("A",1),("B",2),("A",2),("B",3))
val rdd1 = src.parallelize(data,2)
val sortByKeyRDD = rdd1.sortByKey(false,2)
sortByKeyRDD.foreach(println)
```

**输出：**

```text
(B,2)
(B,3)
(A,1)
(A,2)
```

**8）cogroup\(otherDataSet，numPartitions\)：**对两个RDD\(如:\(K,V\)和\(K,W\)\)相同Key的元素先分别做聚合，最后返回\(K,Iterator&lt;V&gt;,Iterator&lt;W&gt;\)形式的RDD,numPartitions设置分区数，提高作业并行度

**9）join\(otherDataSet,numPartitions\):**对两个RDD先进行cogroup操作形成新的RDD，再对每个Key下的元素进行笛卡尔积，numPartitions设置分区数，提高作业并行度

**10）LeftOutJoin\(otherDataSet，numPartitions\):**左外连接，包含左RDD的所有数据，如果右边没有与之匹配的用None表示,numPartitions设置分区数，提高作业并行度

**11）RightOutJoin\(otherDataSet, numPartitions\):**右外连接，包含右RDD的所有数据，如果左边没有与之匹配的用None表示,numPartitions设置分区数，提高作业并行度





























