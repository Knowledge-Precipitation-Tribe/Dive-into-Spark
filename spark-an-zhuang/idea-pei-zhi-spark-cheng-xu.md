# IDEA 配置Spark程序

官网没有介绍这一部分，之前也没使用过IDEA这个软件，所以学习到不少新知识，这里放出我从无到有的配置过程，大神可以直接忽略、

**1、IntelliJ IDEA下载及安装**

    **** 首先依旧是下载，去[官网下载 IDEA](https://www.jetbrains.com/idea/?fromMenu)，这里 选择`Community`版本，这个版本是免费提供的，对我们的Spark使用来说，用这个版本已经足够了。

![](../.gitbook/assets/image%20%2813%29.png)

下载完成后，双击`.exe`文件，开始进行安装，其中所有选项按照默认的即可（其中有一个安装路径的配置，按照普通软件安装的方法自行设置即可，当然使用默认的路径也可以，**记住这里的安装路径，后面要用到**），一路点击`Next`，最后点击`Finish`按钮结束安装过程。  
  
**2、IDEA的配置**

**1）Scala插件的安装**

       ****我们主要用Scala来写Spark程序，而IntelliJ IDEA需要使用Scala插件来支持Scala，安装方法如下图所示。

![](../.gitbook/assets/image%20%285%29.png)

随后我们在搜索框内搜索scala插件，即可出现Scala插件的安装界面，点击右侧页面中的`Install`进行安装后，如下图所示：  


![](../.gitbook/assets/image%20%2817%29.png)

**注意：插件安装完了之后，记得重启一下IntelliJ IDEA使得插件能够生效。**

**2）全局JDK和Library的设置**

因此为了后续创建Spark项目（正如上面所说，一方面是Scala本身需要依赖JDK，另一方面用来管理项目构建的Maven，其创建也需要依赖JDK）的时候不用每次都去配置JDK，这里先进行一次全局配置。 首先在欢迎界面点击`Configure`，然后菜单中选择`Project Structure。`然后在打开的`Default Project Structure`界面的左侧边栏选择`Project`，在右侧打开的页面中创建一个新的JDK选项（一般自动就选择好了）

![](../.gitbook/assets/image%20%2816%29.png)

 这时候选择`Global Libraries`，然后在中间一栏中有一个绿色的加号标志 **`+`**，点击后在下拉菜单中选择 `Scala SDK`（如果没有的话，回顾上面的步骤，仔细观察一下是不是有哪些步骤错了，比如Scala的插件没安装成功，本机还未安装Scala，亦或者Scala的bin文件夹路径未能添加到系统的 **PATH** 环境变量中去等等），然后在打开的对话框中选择系统本身所安装的Scala（即System对应的版本），点击`OK`确定，这时候会在中间一栏位置处出现Scala的SDK，如下图所示，在其上右键点击后选择`Copy to Project Libraries...`，这个操作是为了将Scala SDK添加到项目的默认Library中去。

![](../.gitbook/assets/image%20%2819%29.png)



















































