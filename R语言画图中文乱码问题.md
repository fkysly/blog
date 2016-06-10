最近在学习R语言，在尝试[标签云教程](http://my.oschina.net/enyo/blog/204298?p=1)的时候，发现mac os系统下，画图中的中文字体会出现框框。

乱码情况：
![乱码图](http://7q5a8u.com1.z0.glb.clouddn.com/cc.jpg)

需要对quartz设备进行中文字体的指定：  
`quartz(family='Hiragino Sans GB W3')`

或者对全局的par进行指定：  
`par(family='Hiragino Sans GB W3')`

这已经可以解决quartz预览显示的问题。但是如果是保存为jpeg图片，就需要对wordcloud函数本身指定：  
`wordcloud(mydata$name,mydata$count,c(6,1),random.order=FALSE,color=mycolor,family='Hiragino Sans GB W3')`

正常显示：
![正常显示](http://7q5a8u.com1.z0.glb.clouddn.com/xxx.jpg)