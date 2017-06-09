
情景：我在window上用eclipse写了一个读取apk的manifest经过信息处理后展示在gui界面（swing写的），现在我要把他导出jar包甚至是安装程序让他可以再window、mac和linux上都可以运行。

[TOC]
### 1、java打包成jar包：

![选择项目点击导出](http://oi2e3199v.bkt.clouddn.com/5d8bbf9c630216bc331cb48da49c261b.png?imageView2/2/w/700/h/500)

![选择导出可执行jar包](http://oi2e3199v.bkt.clouddn.com/82932156d01d9eda6b554dddf036980a.png?imageView2/2/w/700/h/500)

![具体设置](http://oi2e3199v.bkt.clouddn.com/62697551c1f548ee1ed36d2d45a48323.png?imageView2/2/w/700/h/500)

![完成结果](http://oi2e3199v.bkt.clouddn.com/e44ba3172cca18affbba1935fed64445.png?imageView2/2/w/700/h/500)


### 2、jar包打成exe文件
使用install4J工具，可以转换成三个平台都可以用的，不过很遗憾的是找不到破解的，你点90天试用会一直弹标语，很难用。

下面是install4J的使用方法（安装就不说了，一直默认就okay）
（安装包我放在网盘里的安装包分类了。install4J 6.x版本。支持64位，jre1.8）

准备工作.
将导出的jar包：ApkAnalytics.jar和lib文件夹，（程序需要的图片：images，数据库database，以及你要生成exe文件后的图标 png图片，如果有的话）全部复制到一个文件夹下面。

![程序名字和版本号](http://oi2e3199v.bkt.clouddn.com/6cc6587865a7a1845bae1dcbdd821455.png?imageView2/2/w/700/h/500)

设置javaVersion 和依赖的环境，下面正常是有三个变量的，全部删掉，点击添加

![javaVersion](http://oi2e3199v.bkt.clouddn.com/bafc601bf493dfef776371176932bdf8.png?imageView2/2/w/700/h/500)

![点击添加后的操作](http://oi2e3199v.bkt.clouddn.com/b56ae3d77a47c6f3fd579578898cd474.png?imageView2/2/w/700/h/500)

![输出的文件夹](http://oi2e3199v.bkt.clouddn.com/c63828ab850a672284c5296fb4ef79ef.png?imageView2/2/w/700/h/500)



接下去next直到Files的第一个选项

![Files-添加jar包](http://oi2e3199v.bkt.clouddn.com/1931ba639bec064aaf752a2e3c048a74.png?imageView2/2/w/700/h/500)


一直next，直到Files的Installation Components

![Installation Components](http://oi2e3199v.bkt.clouddn.com/d541235b3be8a6096e27b2398502acfb.png?imageView2/2/w/700/h/500)

![新建Launcher](http://oi2e3199v.bkt.clouddn.com/be74f23a1070d5bccbc9915ceca99afd.png?imageView2/2/w/700/h/500)

![选择launcher类型](http://oi2e3199v.bkt.clouddn.com/bb6ae9a5687c27089927de11f882490c.png?imageView2/2/w/700/h/500)

![设置程序信息](http://oi2e3199v.bkt.clouddn.com/b56e2295d1e469dae9eb54d2ff54c4d8.png?imageView2/2/w/700/h/500)

![添加图标](http://oi2e3199v.bkt.clouddn.com/c7072e66a1e250a2cc018dda962371f8.png?imageView2/2/w/700/h/500)

![配置java的classpath和main入口](http://oi2e3199v.bkt.clouddn.com/f8fadc9174a71165a99f23065ff2e57b.png?imageView2/2/w/700/h/500)

选择一直next直到Media的第一个选项

![创建meida](http://oi2e3199v.bkt.clouddn.com/3fa1f692fa72e6e4613b9aa8473bafb2.png?imageView2/2/w/700/h/500)

![选择安装包类型](http://oi2e3199v.bkt.clouddn.com/21826a6cdddbdf0b3c554f77d6b06ddf.png?imageView2/2/w/700/h/500)

![设置主程序入口](http://oi2e3199v.bkt.clouddn.com/ef07a5484e75c6831e9d44800ca40440.png?imageView2/2/w/700/h/500)

![绑定jre](http://oi2e3199v.bkt.clouddn.com/6def8ebde7d5cbf0d0c4ad7373f5414f.png?imageView2/2/w/700/h/500)

![排除文件](http://oi2e3199v.bkt.clouddn.com/575a7f8fb6b6a1c730b7fdb62551ca6b.png?imageView2/2/w/700/h/500)

点击finish后

![生成安装包](http://oi2e3199v.bkt.clouddn.com/e583f2df42cfe21b909a9d671cd48051.png?imageView2/2/w/700/h/500)

![转换安装包类型继续生成](http://oi2e3199v.bkt.clouddn.com/471f9474a1a13a08bea3b2ebdea0d974.png?imageView2/2/w/700/h/500)


附录1：手动创建JRE Bundle

![选择创建Bundle](http://oi2e3199v.bkt.clouddn.com/882332fea2031bb4868589a8725f3b31.png?imageView2/2/w/700/h/500)

![具体配置](http://oi2e3199v.bkt.clouddn.com/5b3cb170b7000c2b96f25295c57515b2.png?imageView2/2/w/700/h/500)


附录2：（转自网上,还未实践过）通过jar命令行操作打包jar包


```
1、jar简介

Java归档文件格式(Java Archive, JAR)能够将多个源码、资源等文件打包到一个归档文件中。这样，有如下好处：

安全性
可以对整个jar包的内容进行签名。
减少了下载时间
如果applet被打包成一个jar文件，那么所有相关的资源就可以在一个HTTP transaction中下载完成，而无需为每一个文件新建一个连接。
压缩
减少了磁盘空间的占用。
容易扩展
通过jar这种格式，可以和容易地将自己的程序打包提供给别人使用。
包密封(Package Sealing)
存储在jar文件中的包可以被密封，来保证版本的一致性。密封可以保证一个包中的所有类都来自同一个jar文件。
包版本说明
一个jar包可以存储关于其内容的信息，包括提供商、版本等。
可移植性
处理jar文件的机制是Java平台核心API的标准模块。
From docs.oracle.com

2、jar的使用

JDK自带的打包工具通过jar命令来调用，jar是通过zip格式进行打包的。所以，这个jar工具也可以作为日常的压缩、解压缩工具来进行使用。

2.1 jar命令说明

如果安装了JDK并配置了环境变量，在命令行中输入jar命令，不加任何参数，就可以看到jar命令的使用说明，最下面还包含了两个例子：

jar
用法: jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...
选项:
    -c  创建新档案
    -t  列出档案目录
    -x  从档案中提取指定的 (或所有) 文件
    -u  更新现有档案
    -v  在标准输出中生成详细输出
    -f  指定档案文件名
    -m  包含指定清单文件中的清单信息
    -n  创建新档案后执行 Pack200 规范化
    -e  为捆绑到可执行 jar 文件的独立应用程序指定应用程序入口点
    -0  仅存储; 不使用任何 ZIP 压缩
    -P  保留文件名中的前导 '/' (绝对路径) 和 ".." (父目录) 组件
    -M  不创建条目的清单文件
    -i  为指定的 jar 文件生成索引信息
    -C  更改为指定的目录并包含以下文件
如果任何文件为目录, 则对其进行递归处理。
清单文件名, 档案文件名和入口点名称的指定顺序与 'm', 'f' 和 'e' 标记的指定顺序相同。

示例 1: 将两个类文件归档到一个名为 classes.jar 的档案中:
       jar cvf classes.jar Foo.class Bar.class
示例 2: 使用现有的清单文件 'mymanifest' 并将 foo/ 目录中的所有文件归档到 'classes.jar' 中:
       jar cvfm classes.jar mymanifest -C foo/

摘自<jar命令的帮助文档>
2.2 jar使用举例

jar命令打包时默认会在jar包中添加清单(manifest)文件，如果不想添加，手动指定-M选项：

>jar -cvf HelloWorld.jar HelloWorld.class   #将HelloWorld.class文件打入jar包
    已添加清单
    正在添加: HelloWorld.class(输入 = 427) (输出 = 289)(压缩了 32%)
>jar -tf HelloWorld.jar   #查看归档文件的内容
    META-INF/
    META-INF/MANIFEST.MF
    HelloWorld.class
>jar -xf HelloWorld.jar META-INF/MANIFEST.MF   #解压出其中的META-INF/MANIFEST.MF文件
>type META-INF\MANIFEST.MF   #查看清单文件的内容
    Manifest-Version: 1.0
    Created-By: 1.8.0_51 (Oracle Corporation)
>jar -cvfM HelloWorld.jar HelloWorld.class   #将HelloWorld.class文件打入jar包，不要添加清单文件
    正在添加: HelloWorld.class(输入 = 427) (输出 = 289)(压缩了 32%)
>jar -tf HelloWorld.jar
    HelloWorld.class
>jar -xf HelloWorld.jar   #解压tar文件到当前目录
如果要生成可以运行的jar包，需要指定jar包的应用程序入口点，用-e选项：

>jar -cvfe HelloWorld.jar HelloWorld HelloWorld.class   #创建可以运行的jar包
    已添加清单
    正在添加: HelloWorld.class(输入 = 427) (输出 = 289)(压缩了 32%)
>jar -tf HelloWorld.jar   #查看归档的内容
    META-INF/
    META-INF/MANIFEST.MF
    HelloWorld.class
>type META-INF\MANIFEST.MF   #查看清单文件的内容
    Manifest-Version: 1.0
    Created-By: 1.8.0_51 (Oracle Corporation)
    Main-Class: HelloWorld
>java -jar HelloWorld.jar   #运行jar包
    Hello World!!
3、清单文件MANIFEST.MF

jar打包时，会默认向jar包中添加一个清单文件(MANIFEST.MF)，放在META-INF目录。上面的例子中，可以看到：如果指定了jar包的入口程序，清单文件中就会多一行Main-Class: HelloWorld。实际上，-e选项的作用就是向MANIFEST.MF文件中添加这样一行信息，来声明jar包的入口程序。当然，我们也可以直接修改清单文件的内容。

3.1 修改清单文件的内容

前面已经说了，-m选项可以添加指定清单文件中的清单信息，格式如下：

jar cfm jar-file manifest-addition input-file(s)
注意：

manifest-addition是一个已经存在的文本文件，其中包含着想要往jar包的清单文件中添加的清单信息。
manifest-addition的文件编码格式必须是UTF-8。
清单信息每行后面必须有回车或者换行。(The text file from which you are creating the manifest must end with a new line or carriage return. The last line will not be parsed properly if it does not end with a new line or carriage return.)
清单文件名, 档案文件名和入口点名称的指定顺序与 ‘m’, ‘f’ 和 ‘e’ 标记的指定顺序相同。(前面已经提到了)
3.2 在清单文件中声明入口程序

>type manifest.txt   #查看清单信息的内容
    Main-Class: HelloWorld

>jar -cvfm HelloWorld.jar manifest.txt HelloWorld.class   #添加清单信息到jar包
    已添加清单
    正在添加: HelloWorld.class(输入 = 427) (输出 = 289)(压缩了 32%)
>jar -xvf HelloWorld.jar META-INF\MANIFEST.MF
      已解压: META-INF/MANIFEST.MF
>type META-INF\MANIFEST.MF
    Manifest-Version: 1.0
    Created-By: 1.8.0_51 (Oracle Corporation)
    Main-Class: HelloWorld   #可以看到清单信息成功添加
>java -jar HelloWorld.jar   #这样指定入口程序，生成的jar包照样可以运行。
    Hello World!!
3.3 在清单文件中指定CLASSPATH

在运行java命令的时候，如果指定了-jar选项，那么环境变量CLASSPATH和在命令行中指定的所有类路径都被JVM所忽略。这时，如果一个jar包引用了其它的jar包，解决方案有两个：

java -cp lib\log4j-1.2.14.jar;hello.jar com.dhn.Hello （com.dhn.Hello为主类）
在windows下多个jar之间以分号（;）隔开,最后还需要指定运行jar文件中的完整的主类名。

java -jar hello.jar
这种，需要修改hello.jar中的MANIFEST.MF，通过MANIFEST.MF中的Class-Path 来指定运行时需要用到的其他jar，其他jar可以是当前路径也可以是当前路径下的子目录。多个jar文件之间以空格隔开。
以下面的MANIFEST.MF文件为例

Manifest-Version: 1.0
Main-Class: com.ibm.portalnews.entrance.Main
Class-Path: lib\commons-collections-3.2.jar lib\commons-configuration-1.5.jar lib\commons-lang-2.3.jar lib\commons-logging.jar lib\dom4j-1.6.1.jar lib\jaxen-1.1-beta-7.jar lib\jdom.jar lib\log4j-1.2.14.jar
其中：

Manifest-Version表示版本号，一般由IDE工具（如eclipse）自动生成
Main-Class 是jar文件的主类，程序的入口
Class-Path 指定需要的jar，多个jar必须要在一行上，多个jar之间以空格隔开，如果引用的jar在当前目录的子目录下，windows下使用\来分割，linux下用/分割
文件的冒号后面必须要空一个空格，否则会出错
文件的最后一行必须是一个回车换行符，否则也会出错
From: java -jar classpath心得

3.4 将jar包A引用的其它jar包一同打包到A里面

这个问题，是我一直想的。如果这样，那么将一个程序到处拷贝的话就方便的多了。然而，这是行不通的，因为JDK的类加载器不会加载jar包内包含的其它jar包中的类。

当然，这样想过的人不止我一个，可以通过一些第三方的工具来完成。比如有个工具叫one-jar，感兴趣的可以看下。

3.5 通过清单文件密封包

JAR中的Package Sealing功能是在Java 1.2引入的。在生成Jar文件时我们可以指定是否将整个Jar或者其中某几个Package进行密封，如果是将Jar文件整个进行密封，那就意味着其内所有的Package都被密封了。Package一旦密封，那么Java 虚拟机一旦成功装载密封Package中的某个类后，其后所有装载的带有相同Package名的类必须来自同一个Jar文件，否则将触发Sealing Violation安全异常。

From Java中Package Sealing的探秘之旅
对包的密封方式主要有两种：一种是对jar文件中的某些包进行密封，另一种是密封jar中的所有包。

对某些包密封。

Manifest-Version: 1.0
Created-By: 1.8.0_51 (Oracle Corporation)

Name: com/test/hello/  
Sealed: true
Name: com/test/world/  
Sealed: true  
密封jar中的所有包。

Manifest-Version: 1.0
Created-By: 1.8.0_51 (Oracle Corporation)
Sealed: true
概括来说，有两种情况触发关于Sealing的安全异常：
1. 检查当前试图加载的类对应的Package是否已经被JVM装载且密封。如果已经被装载且密封了，但被密封的Package与当前加载的类对应的Package不是来自同一个Jar文件，将触发安全异常。
2. 检查当前试图加载的类对应的 Package是否已经被JVM装载且密封。如果已经被装载但没有被密封，但当前试图加载的类对应的Package确要试图进行密封操作，将触发安全异常。JVM不允许对已经装载但未密封的Package再进行密封操作。

Package Sealing的好处：
Package Sealing所能带来的好处主要是版本一致性. 我们知道Java 在运行时是严格按照classpath中定义的顺序进行装载和检查，尤其是现在Java开源包满天飞, 很有可能你的Java应用程序或者中间件的classpath中会在不同的Jar文件中包含同一个Package的不同版本。这会使得程序运行产生不一致性结果，很难发现。

From: Java中Package Sealing的探秘之旅


```
