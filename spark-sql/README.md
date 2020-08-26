# Spark SQL

### **Spark SQL的前世**

Spark SQL来自于Shark**\(Hive on Spark\)**，而Shark是参考hive把类 sql 的语句转换成 mapreduce，解决了开发难的问题的方式，因此以 hive 为中心将类 sql 的语句转换成 Spark能出来的RDD的框架Shark就出现了。但是由于hive 的底层还是 mapreduce，因此为了 提高 shark 的效率，spark 自己开发了一套算法，替代了之前 hive 的思路，这套算法就是 sparkSQL。

![](../.gitbook/assets/image%20%2847%29.png)

### **Spark SQL的今生**

由于Shark是完全依赖于Hive，不方便添加新的优化策略，其次Spark是线程级并行，不同于MapReduce的进行并行，因此最终有了**Spark SQL。**同时解释一下，**Spark SQL**不受限于**hive，**只是兼容**hive，**如下图所示；而对于**Hive，**也出现了**Hive on Spark，**即将**Spark**作为**Hive**的底层引擎之一，不在让**Hive**受限于单个引擎，可以采用**Map-Reduce、Tez、Spark。**

![](../.gitbook/assets/image%20%2846%29.png)

上图的Spark SQL架构可以看出，Spark SQL只依赖了HiveSQL解析部分，之后将解析道德一批未被解决的逻辑计划，经过分析得到分析后的逻辑计划，再经过一批优化规则转换成一批最佳优化的逻辑计划，再经过SparkPlanner的策略转化成一批物理计划，随后经过消费模型转换成一个个的Spark任务执行。除此之外SparkSQL为了简化RDD的开发，提高开发效率，提供了DataFrame和DastaSet两个抽象类。

#### DataFrame:

      DataFrame是一种基于RDD为基础的分布式数据集，类似于传统数据库中的二维表格，DataFrame和RDD的主要区别在于前者有Schema信息，即在DataFrame中数据存在列名和数据类型，这使得Spark SQL可以在DataFrame数据集上进行操作。**除此之外DataFrame还支持嵌套数据类型，提供了一套高级的关系操作API。**





### **Spark架构**

### **Spark特点**

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

