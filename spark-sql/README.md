# Spark SQL

### **Spark SQL的前世**

Spark SQL来自于Shark**\(Hive on Spark\)**，而Shark是参考hive把类 sql 的语句转换成 mapreduce，解决了开发难的问题的方式，因此以 hive 为中心将类 sql 的语句转换成 Spark能出来的RDD的框架Shark就出现了。但是由于hive 的底层还是 mapreduce，因此为了 提高 shark 的效率，spark 自己开发了一套算法，替代了之前 hive 的思路，这套算法就是 sparkSQL。

![](../.gitbook/assets/image%20%2863%29.png)

### **Spark SQL的今生**

由于Shark是完全依赖于Hive，不方便添加新的优化策略，其次Spark是线程级并行，不同于MapReduce的进行并行，因此最终有了**Spark SQL。**同时解释一下，**Spark SQL**不受限于**hive，**只是兼容**hive，**如下图所示；而对于**Hive，**也出现了**Hive on Spark，**即将**Spark**作为**Hive**的底层引擎之一，不在让**Hive**受限于单个引擎，可以采用**Map-Reduce、Tez、Spark。**

![](../.gitbook/assets/image%20%2862%29.png)

上图的Spark SQL架构可以看出，Spark SQL只依赖了HiveSQL解析部分，之后将解析道德一批未被解决的逻辑计划，经过分析得到分析后的逻辑计划，再经过一批优化规则转换成一批最佳优化的逻辑计划，再经过SparkPlanner的策略转化成一批物理计划，随后经过消费模型转换成一个个的Spark任务执行。除此之外SparkSQL为了简化RDD的开发，提高开发效率，提供了DataFrame和DastaSet两个抽象类。

#### DataFrame:

      DataFrame是一种基于RDD为基础的分布式数据集，类似于传统数据库中的二维表格，DataFrame和RDD的主要区别在于前者有Schema信息，即在DataFrame中数据存在列名和数据类型，这使得Spark SQL可以在DataFrame数据集上进行操作。**除此之外DataFrame还支持嵌套数据类型，提供了一套高级的关系操作API。**

### DataSet

      DastaSet是一个新的分布式数据集合，是Spark1.6新加的抽象，是DataFrame的一个扩展。它提供了RDD的优势以及Spark SQL的优化执行引擎的优点。

### **Spark架构**

### **Spark特点**

#### 1 集成

无缝地将SQL查询与Spark程序混合。 Spark SQL允许您将结构化数据作为Spark中的分布式数据集\(RDD\)进行查询，在Python，Scala和Java中集成了API。这种紧密的集成使得可以轻松地运行SQL查询以及复杂的分析算法。

#### 2 统一数据访问

加载和查询来自各种来源的数据。 Schema-RDDs提供了一个有效处理结构化数据的单一接口，包括Apache Hive表，**HDFS**文件和JSON文件。

#### 4.3 Hive兼容性

在现有仓库上运行未修改的Hive查询。 Spark SQL重用了Hive前端和MetaStore，为您提供与现有Hive数据，查询和UDF的完全兼容性。只需将其与Hive一起安装即可。

#### 4.4 标准连接

通过JDBC或ODBC连接。 Spark SQL包括具有行业标准JDBC和ODBC连接的服务器模式。

#### 4.5 可扩展性

对于交互式查询和长查询使用相同的引擎。 Spark SQL利用RDD模型来支持中查询容错，使其能够扩展到大型作业。不要担心为历史数据使用不同的引擎。

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

