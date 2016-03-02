---
layout:	post
title: 在iOS开发中使用iconfont
date: 2016-02-29 14:44:33 +0800

---

在开发寓悦app的时候，老大提出使用iconfont来替代传统的切图，于是乎开始了get新技能之旅。


##瓦特？iconfont是什么鬼

我所理解的iconfont，就是将.svg格式的图片打包成.ttf格式的字体，用来展示图片或者特殊字符等。理解有限，需要详细了解svg以及iconfont的小伙伴们请自行戳文末链接。

##制作iconfont
扯了这么多，终于要开始了我们的iconfont之旅了，激动的心颤抖的手啊

[iconmoon][iconmoon]、[Fontello][fontello]、[阿里巴巴矢量图标库][iconfont.cn]等网站都提供了生成iconfont的服务，本文以阿里巴巴矢量图标库（以下简称图标库）作为生成工具。

[iconfont.cn]: http://www.iconfont.cn/
[easyicon]: http://www.easyicon.net/
[iconmoon]: https://icomoon.io/
[fontello]: http://fontello.com/

首先上传icon，上传成功后的icon可以在我上传的图标里看到，点击选择我们需要打包的icon，存储为项目，存储时可以存储为新的项目，往历史项目里添加icon的话就选择存储为历史项目，然后点击项目图标管理，下载至本地就ok啦。

![photo](/Resource/2016-03-02/1.png)

##使用iconfont

###1、添加iconfont字体

将下载好的字体（.ttf）添加到工程中，打开Info.plist文件，添加新属性`Fonts provided by application`，增加字体名`iconfont.ttf`。

![photo](/Resource/2016-03-02/2.jpg)

###2、使用iconfont字体

奉上iconfont官网使用说明

![photo](/Resource/2016-03-02/3.png)

什么？还需要找每个Unicode编码，太麻烦啦。我们的做法是将Unicode编码都写成静态字符串（如下图），使用起来比较方便。

![photo](/Resource/2016-03-02/4.png)

转换脚本奉上，每次修改了项目图标的时候点击Font，获取在线链接把链接替换掉，再运行下脚本粘贴到工程里对应的文件就好了。

iOS生成脚本

	curl -v at.alicdn.com/t/font_1456369942_1967018.css|pcregrep --om-separator='=@"\\U0000' -o1 -o2 '.icon-(.*?):before { content: "\\(.*?)";' |while read line;do echo "static NSString *kIcon_${line}\";";done|pbcopy

安卓生成脚本

	 curl -v at.alicdn.com/t/font_1450322861_4570909.css|pcregrep --om-separator='">\\u' -o1 -o2 '.icon-(.*?):before { content: "\\(.*?)";' |while read line;do echo "<string name=\"${line}</string>";done|pbcopy
	 
	

新技能get~(≧▽≦)/~啦啦啦 

 

##Reference
关于svg: <www.iosxxx.com/blog/2015/10/07/ioszhong-de-svgyu-iconfontxiang-jie/>

关于iconfont:<https://www.zybuluo.com/cherishpeace/note/2080>

iconfont平台介绍: <http://www.iconfont.cn/help/platform.html>


