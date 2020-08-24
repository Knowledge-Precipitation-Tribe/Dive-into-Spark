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

![](../.gitbook/assets/image%20%2841%29.png)

**8）10.coalesce\(numPartitions，shuffle\)**对RDD的分区进行重新分区，参数一：分区数；参数二：是否进行打乱。怎么理解参数二呢？coalesce是将分区数减少到numPartitions个分区，

shuffle默认值为false,当shuffle=false时，不能增加分区数目,但不会报错，只是分区个数还是原来的





















































