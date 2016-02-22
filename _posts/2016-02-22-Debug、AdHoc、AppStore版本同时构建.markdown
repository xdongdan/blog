---
layout:	post
title: Debug、AdHoc、AppStore版本同时构建
date: 2016-02-22 15:44:33 +0800
---
###介绍

去年临过年的时候测试老是抱怨手机上只能安装debug、adhoc、release某一个版本，每次安装跟设备上版本不一样的包的时候就会把原来的包覆盖掉。趁着今天无聊看了下不同版本的构建，将整个过程记录了下来，希望能帮助到和我一样整天被测试抱怨的小伙伴。

###1、创建不同的App ID

要构建不同版本的app，首先需要创建不同的App ID。.debug、.adhoc分别指向debug、adhoc版本，没有后缀名的是用于指向AppStore版本的。

	com.houpix.test   
	com.houpix.test.debug  
	com.houpix.test.adhoc 	
	
App ID创建好以后就可以创建对应的描述文件了，分别创建AppStore、AdHoc、development的描述文件。

###2、项目配置

Xcode默认会创建两个构建配置：Debug和Release，选择PROJECT-->Info-->Configurations来增加第三个：Ad Hoc用来内部分发，当然也可以直接真机调试。

![photo](/Resource/2016-02-22/1.png)

记得添加的时候要选择“Release”哦。


接下来我们来为BundleID和BundleName添加后缀，选择TARGET-->Info。不出意外的话你的BundleID显示的会是${PRODUCT_BUNDLE_IDENTIFIER}，我们需要把他改成`com.houpix.test${BUNDLE_ID_SUFFIX}`；同样如果没设置过你的BundleName会显示的是${PRODUCT_NAME}，改为`测试${BUNDLE_NAME_SUFFIX}`。

![photo](/Resource/2016-02-22/2.png)


然后选择Bulid Settings-->+-->Add User-Defined Setting来添加User-Defined，这里会分别添加`BUNDLE_ID_SUFFIX`和`BUNDLE_NAME_SUFFIX`与上面修改的对应起来，添加好后展开会有AD Hoc、Debug、Release。

BUNDLE_ID_SUFFIX必须与你在创建的App ID对应，我们在Ad Hoc那一栏填写.adhoc，在Debug那一栏填写.debug，Release不设置，这样在应用Build的时候后缀会附加到BundleID后面。

同上我们在BUNDLE_NAME_SUFFIX里填写应用名称的后缀。我这里分别填写的 A和 D来对应Ad Hoc和Debug。

![photo](/Resource/2016-02-22/3.png)

最后，需要设置Code Signing，小伙伴们都是身经百战被虐无数次的，这一步就不多说了，看图。

![photo](/Resource/2016-02-22/4.png)

###3、Schemes

我们的应用一开始有一个默认的scheme，Run、Test、Analyze是Debug，Archive是Release，我们需要再创建一个AdHoc的scheme，Run、Test、Archive都使用AdHoc配置。这样的话，我们就可以在Xcode里选择不同的scheme来构建应用了。分别运行一下，完美！(ps:这里我有三个scheme，所以运行出来是三个应用。)

![photo](/Resource/2016-02-22/5.png)


###Reference
<http://www.swwritings.com/post/2013-05-20-concurrent-debug-beta-app-store-builds/>

翻译版：<http://joakimliu.github.io/2015/07/01/Translate-The-Blog-Post-Concurrent-Debug-Beta-and-App-Store-Builds/>

