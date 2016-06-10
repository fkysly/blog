# Scala从零单排——拉开序幕

### Scala介绍
Scala由Martin Odersky教授进行主导设计，是新一代的高级程序设计语言。Scala语言功能强大，包含面向对象、函数式、元编程等多种编程范式，吸纳结合了多种常见编程语言的特性，当然，这也增加了其本身的语言复杂程度。  
Scala构建于JVM之上，依托于JVM平台的类库和扩展，快速成长，当然开源社区也相继推出Scala.js和Scala Native等平台扩展产物。

### Scala环境搭建
#### Scalac
单纯从Scala角度来说，直接下载官方提供的二进制安装包，或者通过`brew install scala`命令，都可以方便的搭建环境。然后就可以使用scalac命令，对程序代码进行编译。非常类似于Java的开发方式。

#### sbt
对于复杂度较高的项目，单纯依赖于官方的编译工具，很难达到预期的效果。社区提供了sbt构建工具，类似于Java平台的Maven。
sbt工具也可以直接通过官网进行下载，或者通过`brew install sbt`，直接安装。（这里不需要提前搭建scala环境，只安装sbt即可）

由于国内网络情况，sbt工具会下载非常缓慢，出现假死情况。可以通过以下手段进行Hack：
* 使用翻墙工具，对Terminal进行代理。
* 配置sbt repositories，更换开源中国的源。
* 配置repox。

笔者采用第三种方式。repox是开源社区的产物，一个公共的源服务器，不仅对sbt下载进行了优化，还支持了更多的文件格式。
* 配置方法参考第二小节：[repox入门指南](https://github.com/Centaur/repox/wiki/%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97)
* 最新社区服务器地址参考：[repox官网](http://centaur.github.io/repox/)

当然，此服务器由民间制作，成本较高，希望各位可以量力捐款。

### Scala项目构建
本文以一个简单的HTTP访问百度，获取200返回码的示例，来展示Scala的项目构建过程。

先建立http文件夹，并且新建build.sbt文件，加入以下代码：
```Scala
name := "http"
version := "1.0"
scalaVersion := "2.11.8"
libraryDependencies ++= Seq(
  "org.scalaj" %% "scalaj-http" % "2.3.0"
)
```
第一行是项目的名称。  
第二行是项目的版本号。  
第三行是指定Scala的版本。  
第四行是配置项目的第三方依赖。[scalaj-http](https://github.com/scalaj/scalaj-http)是一个Scala的http封装。

根目录新建src/main/scala嵌套文件夹，并且在最内层scala文件夹下新建main.scala文件，加入以下代码：
```Scala
package com.example.main
import scalaj.http._

object Main {
  def main(args: Array[String]) {
    val response: HttpResponse[String] = Http("http://baidu.com").asString
    println(response.code)
  }
}
```
第一行是设定包名。  
第二行是导入scalaj-http包，_表示引入所有。  
第四行是建立Main对象，作为程序的入口对象。  
第五行是建立main方法，作为程序的入口方法。  
第六行是利用scalaj-http包的HTTP函数获取响应内容。  
第七行是输出响应的返回码。

然后打开Terminal，输入sbt，等待...，输入run，可以看到结果是200。

### Scala一日游感受
总体来说，Scala语言非常的酷，不乏函数式的简洁和面向对象的方便。相应的学习资料不是很多，遇到工程问题比较需要耐心，最好有Java的基础。
