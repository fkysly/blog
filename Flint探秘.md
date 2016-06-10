# Flint探秘

### 前言

前端逐年发展，从实现刀耕火种的纯静态页面，到开发复杂交互的前端应用，各种框架、自动化工具层出不穷。在国内的前端界忙着造 MVVM 的轮子、为选择哪家框架而斗嘴的时候，国外的团队已经开始思考开发体验的优化了。

手淘前端内部目前比较流行的工作流，是 MVVM 框架 + 各种第三方库 + NPM 包管理 + Gulp 流 + Webpack 打包 + 本地服务器 + Hot Loader 插件或者是 LiveReload 插件，再配合上喜欢的编辑器、浏览器。从上古时期的写完 HTML 、 CSS 、 JavaScript ，直接浏览器刷新，慢慢发展到今天需要编译的自动化工作流，前端开发的能力上了一个台阶的同时，复杂性也逐年升高，越来越“折腾”。

如果你跟我一样不想再“折腾”下去，可以随着我一起了解一下 Flint 。它是国外团队最近的一个作品，还在Beta当中，没有公开发布。在这篇文章当中，我会逐步从浅到深，聊一聊这款框架的特性和脑洞。

PS: 在本文中，由于Flint 本身的语法以及编译不是重点，侧重于表达特性和原理，所以对于这两部分不作介绍。

------

### Flint 介绍

#### 什么是 Flint

![首页介绍图](http://gw.alicdn.com/mt/TB1kbbyKFXXXXaGXXXXXXXXXXXX-962-563.png_500x500.jpg)

据官网所说， Flint 是一个前端的编译器，它连接了编辑器和浏览器，多种特性让开发 Web 应用更加高效、快捷。第一呢， Flint 目前还在马不停蹄地开发当中，官网、代码说变就变，说不定笔者发稿的时候，官网的描述又变动了；第二呢，据源码分析，编译只是 Flint 项目的一个部分，它其实包含了服务器、命令行、工作流等等多种东西，所以综合来说， Flint 是一个智能的前端开发环境。

#### Flint 使用

安装 Flint 很简单，可以通过npm安装：

``` 
npm install -g flint
```

安装完成之后，Flint 有三个命令很重要：

``` 
flint new
flint run / flint
flint build
```

分别是新建项目，运行项目和构建项目。

我们首先来尝试新建一个项目，并且进入项目运行它：

``` 
flint new hello
cd hello & flint
```

![Flint运行图](http://gw.alicdn.com/mt/TB12O2hKFXXXXbeXFXXXXXXXXXX-622-152.png_200x100.jpg)

可以见到项目已经跑起来了， Flint 为我们运行了一个本地的服务器，注意命令行的最下方， Flint 为我们设置了4个快捷键，现在键盘按 O 键，打开浏览器就可以看到一个 "Hello World" 呈现在眼前了。

![浏览器helloworld](http://gw.alicdn.com/mt/TB1l4bGKFXXXXXcXXXXXXXXXXXX-348-204.png_200x100.jpg)

#### $EDITOR 配置

上文的4个快捷键中的 Editor 快捷键，是跟系统的 $EDITOR 环境变量挂钩的。所以如果你系统默认没有配置这个变量，上文那里会显示错误警告。这里不再赘述 $EDITOR 的配置，请参考相关资料。

#### 插件配置

官方希望在 Atom 上完成整个 Flint 生态的闭环，建立编辑器与浏览器之间的连接，这个就需要编辑器的插件来配合。根据官方的 issue 来看，正在研发 Atom-Flint 插件，随后开始 Sublime&Vim 上插件的开发。所以，如果你希望更好的体验 Flint 带来的乐趣，下载一个 Atom 编辑器，安装上 Atom-Flint 这个插件。（不过此插件目前不太稳定，容易报 Bug ）可以体验的一点是，如果你的代码出错，浏览器热更新的同时，你的编辑器也会定位到哪行出错，进行提示。

经过笔者的尝试，目前大部分特性都还没有实现，此文不深入介绍。

![Atom-Flint](http://gw.alicdn.com/mt/TB1D9LgKFXXXXc1XFXXXXXXXXXX-597-269.png)

#### 项目目录解析

Flint 出了新建项目一眼可以看到的 main.js 文件之外，还有一个核心的隐藏目录 .flint 。这个目录是整个Flint运行时的内容，结构如下图所示：

![编译前](http://gw.alicdn.com/mt/TB188PvKFXXXXX1XpXXXXXXXXXX-1058-1008.png_300x300.jpg)

package.json 文件对应 node 的标准配置文件

flint.json 文件对应 Flint 自身的配置文件

index.html 文件对应项目的主页面

static 目录对应项目的静态资源文件目录



接着，让我们来了解一下 Flint 编译之后的情况：

``` 
flint build
```

![编译后](http://gw.alicdn.com/mt/TB1UzfAKFXXXXccXXXXXXXXXXXX-1050-994.png_300x300.jpg)

编译之后的目录，与之前的相比，多了 build 目录，其中存放了编译之后产生的主页面（添加了相关资源文件的引用）， _ 目录里面存放了运行时所需要的 Flint&React&App 源码，以及样式文件。

------

### 特性总览

#### 路由

Flint 自带简单的路由功能，从而能够支持前端完成简单的 SPA 应用，这是非常赞的，不需要添加任何插件

#### NPM 包自动安装 & 本地 import

重新使用 run 命令运行项目，并打开 main.js 文件，在最上方加入：

``` 
import jquery from 'jQuery'
```

然后保存，观察命令行，提示你 jquery 安装成功。进入刚刚提到的 .flint 目录，发现多了 node_modules 目录，这个是 npm 包的安装目录，其中就有刚刚导入的 jQuery 包。这就是 Flint 一个非常令人兴奋的特性—— NPM 包自动安装。由于 Flint 是 ES6 语法，只要你按照规范，在 Flint run 时使用 import 引入你想要的远程包，它就会自动替你把想要引入的包下载到目录里面，这样你无感知的就可以使用了。

经过源码分析和测试， Flint 也同样支持 require 语法自动引入；也支持 import 本地的 npm 包。

#### 超级热更新

Flint 另外一个令人兴奋的特性，就是它集成了热更新技术。相信如果经常使用 Webpack 的开发者，应该已经尝到了热更新技术的甜头。它能够使得在不刷新浏览器的情况下，更改本地的前端代码（组件），浏览器自动更新预览。 Flint 直接集成了这项技术，而且建立了专门的通道进行错误的实时反馈。当然，其实 Flint 做编辑器插件，不仅是建立报错的连接，更重要的是将社区最新的 "hot update on save" 推进到更新的 "hot update on type" ，能够真正做到敲代码的同时，项目及时编译反馈到浏览器预览，此特性仍待观察。

#### 给力的样式

![给力样式](http://gw.alicdn.com/mt/TB1uKS.KFXXXXb4XVXXXXXXXXXX-1372-530.png_400x400.jpg)

Flint 使用 $ 符号作为选择器，不仅支持左图所示的静态样式，也支持右图的动态样式，支持简单的样式表达式。根据文档描述，静态的样式会在编译过程中，被抽取出来，直接转化为静态的 CSS ，加速性能。

------

### 深入分析

#### 命令行

Flint 提供了三个主要的命令， new&run&build。分别是新建项目、运行项目和构建项目，正好是整个开发的一套流程。

对于 new 命令来说，支持 -u 参数，描述是 "start with a scaffold" ，使用这个参数，可以新建一些脚手架，快速形成项目结构。通过源码分析，是直接从 Github 上拉取的。

对于 run 命令来说，背后支持的是 Flint 的专门的一个 runner 项目，这是一个开发的运行时，主要进行了本地服务器的搭建、 Gulp 工作流的编译、包的安装与卸载、一个传送消息的 bridge 。

对于 build 命令来说，也支持一个 -w 的参数，也就是日常的 watch ，会对项目进行持续的构建。背后也是 runner 项目在支持，只不过这次会把 Flint.js 拷贝到项目目录中，从而可以脱离 runner ，在生产服务器上运行 compiler 之后的项目。

![build命令](http://gw.alicdn.com/mt/TB1zN2BKFXXXXXjXpXXXXXXXXXX-763-174.png)

#### Gulp 编译流程

上文提到的 runner 的核心其实是一个 Gulp 编译流程：

![源码1](http://gw.alicdn.com/mt/TB1SlfaKFXXXXXFaXXXXXXXXXXX-564-425.png)

通过 Babel 去转换源码，把自己的 transform 写成一个 Babel 的插件，去按自己的喜好编译项目：

![源码2](http://gw.alicdn.com/mt/TB1y_zkKFXXXXcUXFXXXXXXXXXX-767-439.png)

可以看到 flintTransform 作为一个插件配置到了 babel 对象上。

PS：可以看到 Gulp 的那段源码的最后一句，一连串写了很多的 .pipe(pipefn()) ，可能是为以后更多的命令留空吧，亦或者是怕 Gulp 处理有问题？求不吝赐教。

#### NPM 包自动下载

关于包管理，内部是通过正则表达式匹配项目中的源码 .js 文件， 看看有没有满足的 import 或者 require 语句，然后将其添加到项目目录中的 package.json ，并且调用 npm install 进行安装， Webpack 对项目打包；相反，检测到包变更，通过 npm uninstall 进行卸载。亲测如果擅自修改 package.json 或者增改 node_modules 的包，很容易就会出错。

![require](http://gw.alicdn.com/mt/TB1gSYjKFXXXXXyXVXXXXXXXXXX-703-41.png)

Flint 内部对包的管理是分为外部引用和内部导出的。由于支持了 import 本地的包，所以专门有个 internal 机制针对本地 exports 出来的包进行管理。

![exports](http://gw.alicdn.com/mt/TB1bITtKFXXXXXkXFXXXXXXXXXX-757-124.png)

#### 服务器

关于服务器，其实 Flint 是内部起了一个基于 Express 的服务器，外加 WebSocket 进行消息的通讯。

------

### 总结

#### Flint 带来的启示

本文大概探索了 Flint 整体的特性和一些原理分析，最令人兴奋的是作者团队无比大的脑洞，为我们带来了很多新的启示，比如连接浏览器与编辑器，比如超级热更新，比如 NPM 包的自动下载等等。目前手淘前端团队的工具链、组件库都已经能够胜任生产工作，基本能够覆盖整体链路。当然在关注更大更全的覆盖率的同时，我们也要静下来思考，开发体验流程上是否简单、智能化。 Flint 带来了一些新的思路，希望可以在未来将这些应用在我们日常的开发当中。