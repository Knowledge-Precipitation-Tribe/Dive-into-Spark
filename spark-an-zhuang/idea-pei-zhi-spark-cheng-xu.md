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

**3）创建Maven工程**

到这一步就开始创建Maven工程了，通过Maven对项目依赖到的Jar包进行理。

在欢迎界面点击`Create New Project`，在打开的页面左侧边栏中，选择`Maven`，然后在右侧的`Project SDK`一项中，查看是否是正确的JDK配置项（如果每一步严格按照上文中的步骤操作的话，正常来说这一栏会自动填充的，如果这里没有正常显示JDK的话，可以点击右侧的`New...`按钮，然后指定JDK安装路径的根目录即可），然后点击`Next`，来到Maven项目最重要三个参数的设置页面，这三个参数分别为：`GroupId`, `ArtifactId`和`Version这三个属性需要你点击`ArtifactId的小箭头，设置好这三个属性和项目名以及项目位置以后可以进入项目页面了。  


进来之后你会看到如下界面，接下来在main文件夹中建立一个名为 `scala` 的文件夹，并右键点击 `scala` 文件夹，选择 Make  Directory as，然后选择`Sources Root`，这里主要意思是将 `scala` 文件夹标记为一个源文件的根目录。

![](../.gitbook/assets/image%20%2822%29.png)

接下来在`scala` 文件夹 上，右键选择 `New`，然后选择 `Scala Class`，随后设置好程序的名称，并且记得将其设置为一个 `Object`，然后就可以看到IntelliJ IDEA自动添加了一些最基本的信息；在创建的 `Object` 中输入如下语句：

```text
def main(args: Array[String]):Unit = {
  println("Spark Hello World!")
}
```

**至此关于IDEA配置Spark就创建完成了！**















































