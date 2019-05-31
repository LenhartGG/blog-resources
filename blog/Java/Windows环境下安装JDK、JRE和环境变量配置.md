# Windows环境下安装JDK、JRE和环境变量配置，详细的图文教程
## 一、准备工具:



### 1.JDK  

JDK 可以到官网下载
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

 

### 2.系统

我这里是WIN7 SP1X 64系统

### 3.根据系统的版本下载相对应的JDK。


我这里选择：jdk-7u80-windows-x64.exe
注意区分：

Java SE Development Kit 8u25
Java SE Development Kit 8u25 Demos and Samples Downloads  
JavaFX Demos and Samples Downloads


第一个 java se开发包
第二个 java se开发包+示例
第三个 javaFX开发包和示例

第一个是必须的配置Java开发环境的

## 二、方法/步骤

 

### 1.安装JDK，JRE， 选择安装目录


安装过程中会出现两次 安装提示 。第一次是安装 jdk ，第二次是安装 jre 。建议两个都安装在同一个java文件夹中的不同文件夹中。（不能都安装在java文件夹的根目录下，jdk和jre安装在同一文件夹会出错）。

**注意：**

{安装过程中会出现两次 安装提示 。第一次是安装 jdk ，第二次是安装 jre 。建议两个都安装在同一个java文件夹中的不同文件夹中。（不能都安装在java文件夹的根目录下，jdk和jre安装在同一文件夹会出错）}。


JDK与JRE安装完成


### 2.配置系统环境

 
配置环境变量：右击“我的电脑”-->"高级"-->"环境变量"。

**（1）JAVA_HOME环境变量。**

作用：它指向jdk的安装目录，Eclipse/NetBeans/Tomcat等软件就是通过搜索JAVA_HOME变量来找到并使用安装好的jdk。

配置方法：在系统变量里点击新建，变量名填写JAVA_HOME，变量值填写JDK的安装路径。（根据自己的安装路径填写）

D:\Program Files\Java\jdk1.7.0_80

**（2）CLASSPATH环境变量。**

作用：是指定类搜索路径，要使用已经编写好的类，前提当然是能够找到它们了，JVM就是通过CLASSPTH来寻找类的。我们需要把jdk安装目录下的lib子目录中的dt.jar和tools.jar设置到CLASSPATH中，当然，当前目录“.”也必须加入到该变量中。
配置方法：
新建CLASSPATH变量，变量值为：`.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;` 。CLASSPATH变量名字，可以大写也可以小写。注意不要忘记前面的点和中间的分号。且要在英文输入的状态下的分号和逗号。

CLASSPATH  ：`.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`

**（3）path环境变量**

 

作用：指定命令搜索路径，在i命令行下面执行命令如javac编译java程序时，它会到PATH变量所指定的路径中查找看是否能找到相应的命令程序。我们需要把jdk安装目录下的bin目录增加到现有的PATH变量中，bin目录中包含经常要用到的可执行文件如javac/java/javadoc等待，设置好PATH变量后，就可以在任何目录下执行javac/java等工具了。

在系统变量里找到Path变量，这是系统自带的，不用新建。双击Path，由于原来的变量值已经存在，故应在已有的变量后加上“`;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin`”。注意前面的分号。

Path：`;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;`(粘贴这个路径)

然后点击确定完成。

## 三、 测试环境。

检验是否配置成功 运行cmd 分别输入java，javac， java -version （java 和 -version 之间有空格）。

打开cmd（Windows+r） ，输入：java 

也可以在命令行输入  echo %JAVA_HOME% 来查看当前的javahome路径。

**注意：**

>**此处JAVA_HOME 环境变量应该指向 JDK 安装路径，不是JRE的安装路径。JDK安装路径应该包含 bin 目录，该目录下应该有 javac.exe、native2sacii.exe等程序；JDK的安装路径下面应该还有bin目录，该目录下有dt.jar 和 tools.jar 两个文件；JDK安装路径下应该包含 jre目录，这个 jre 就是 JDK 自带的 JRE。**