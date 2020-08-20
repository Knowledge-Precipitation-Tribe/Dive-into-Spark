# Windows 安装Spark

由于spark是用Scala写的，所以Spark对Scala肯定是原生支持的，同时Spark也是基于java环境开发的，因此这里主要以Scala为主来介绍Spark环境的搭建，主要的步骤包括四步，分别是：JDK的安装，Scala的安装，Spark的安装，Hadoop的下载和配置。

**1、JDK的安装**

        由于JDK的安装比较基础，这个就跳过了，不过要说明一点要安装jdk1.8的版本。下载地址 [Java SE Downloads](https://link.jianshu.com/?t=http://www.oracle.com/technetwork/java/javase/downloads/index.html) ,选择如图的版本即可。

![JDK&#x7248;&#x672C;](../.gitbook/assets/image%20%289%29.png)

**2、Scala的安装**

   ****    首先从[官网](https://link.jianshu.com/?t=http://www.scala-lang.org/download/all.html)下载对应的版本，这里面有很多版本，具体选哪个呢，这个需要你根据你安装的spark安装的版本来选，什么意思呢？Spark的各个版本需要跟相应的Scala版本对应，比如我这里使用的Spark 2.2.0 就只能使用Scala 2.11的各个版本。这一点官网给出了对应表，如下所示：

> 请注意，Spark 2.x是用Scala 2.11预先构建的，而版本2.4.2是用Scala 2.12预先构建的。Spark 3.0+是使用Scala 2.12预先构建的。

以我为例，我应该下载Scala 2.11的版本，根据官网找到windows的安装文件，以msi结尾。

![Scala 2.11&#x4E0B;&#x8F7D;](../.gitbook/assets/image%20%287%29.png)

下载好Scala的msi文件后，可以双击执行安装。安装成功后，默认会将Scala的bin目录添加到**PATH**系统变量中去（如果没有，和JDK安装步骤中类似，将Scala安装目录下的bin目录路径，添加到系统变量**PATH**中，和JDK全局环境配置一样），打开cmd验证是否正确安装，输入scala，如下所示，表明安装正确。

![](../.gitbook/assets/image%20%286%29.png)

如果不能显示版本信息，并且未能进入Scala的交互命令行，可能存在原因，一Path系统变量中未能正确添加Scala安装目录下的bin文件夹路径，二Scala未能够正确安装，重复上面的步骤即可。

**3、Spark的安装**

Saprk的安装比较简单，首先还是从官网进行下载 [Download Apache Spark](https://link.jianshu.com/?t=http://spark.apache.org/downloads.html)。这里也要注意**Spark和Hadoop的版本也要对应上。**进入官网可以自己进行选择，如下图所示，自己选择好对应的版本，直接点击3处链接下载。

![](../.gitbook/assets/image%20%2811%29.png)

如果想下载之前的版本，如下图所示，可以到官网末尾选择之前的版本，注意这里下载 后缀为 .tgz 的文件。 

![](../.gitbook/assets/image%20%2812%29.png)

下载完成之后，将文件解压，将Spark的bin目录添加到系统变量**PATH**中。例如我这里的Spark的bin目录路径为`D:\Spark\bin`，那么就把这个路径名添加到系统变量的**PATH**中即可，方法和JDK安装过程中的环境变量设置一致，设置完系统变量后，在任意目录下的cmd命令行中，直接执行`spark-shell`命令。

这时候去cmd运行`spark-shell`命令，很有可能会碰到各种错误，这里主要是因为Spark是基于Hadoop的，所以这里也有必要配置一个Hadoop的运行环境。

**4、Hadoop的安装**

首先依旧是去官网下载，这里给出Hadoop的各个历史版本下载地址[Download Hadoop](https://archive.apache.org/dist/hadoop/common/)，这里注意下载和Spark对应的版本即可。

![Hadoop&#x4E0B;&#x8F7D;](../.gitbook/assets/image%20%2810%29.png)

下载并解压到指定目录，然后到环境变量部分设置**HADOOP\_HOME**为Hadoop的解压目录，我这里是 D:`\hadoop`，然后再设置该目录下的bin目录到系统变量的**PATH**下，我这里也就是D:`\hadoop\bin`，如果已经添加了**HADOOP\_HOME**系统变量，也可以用`%HADOOP_HOME%\bin`来指定bin文件夹路径名。这两个系统变量设置好后，开启一个新的cmd，然后直接输入`spark-shell`命令。

这时候可以进cmd通过spark-shell来启动 spark了，但是我遇到空指针的错误。**主要是因为Hadoop的bin目录下没有winutils.exe文件的原因造成的，**于是需要从[https://github.com/steveloughran/winutils](https://github.com/steveloughran/winutils) 下载对应Hadoop版本号的winutils.exe文件，放到Hadoop解压文件下的bin文件夹中,我这里是D:`\hadoop\bin.`



























































