---
title: 移动端web页面调试方法
date: 2017-05-07 21:54:56
tags:
- js
- web调试
categories:
- web调试
---
<h1 style="color:green;width: 100%;text-align: center ">移动端web调试方法</h1>
### *前言


<!-- more -->
> <div style="color:#D00750">移动端调试不同于桌面端调试，在桌面时代，chrome的调试器已经无比强大。但是在手机上调试web页面就没有这么原始的方式了。
  本文就如何调试移动端web页面，借鉴了一些博文和目前市面上好用的工具，针对移动端web页面以及APP内嵌webview页面如何调试做了一个试用和整理。
  先谈谈需求。
  最理想的方式是什么？</div>

#### 一.ios设备下
1. H5页面
2. webview页面

#### 二.安卓设备
1. H5页面
2. webview页面


 最理想的状态就是在上述四种情况下，H5页面在各种浏览器下都可以进行调试，且调试的方式跟PC chrome类似，包含js断点调试。webview页面在微信以及公司的APP产品内可以进行类似PC chrome类似的调试方式，包含js断点调试。


--------------------------------
--------------------------------
### <span style="color:red">IOS 设备</span>
> 1、 ios safari + mac safari + iphone真机
#### 调试safari浏览器的H5页面
启用功能：

手机端：设置 → Safari → 高级 → Web 检查器 → 开。

mac端：Safari → 偏好设置 → 高级 → 在菜单栏中显示“开发”菜单。

然后就可以在电脑端调试iphone上的safari浏览器上的样式。在调试器及资源里可以对js打断点。操作方式跟chrome的调试器一样。不同的是我们是在手机上对页面进行操作，触发断点环境会更真实。

若在js中埋入一些console，在IOS真机上执行一些操作，mac端safari上调试器能打印，这能极大的方便复杂手势的一些操作。

#### 优点：包含js断点功能，调试方式跟桌面端chrome几乎一样
#### 缺点：只能在手机上的safari浏览器上操作，不能覆盖其他手机环境，另外也不支持webview的调试。
> 2、 万金油 weinre + 真机
#### 调试任意浏览器的H5页面以及webview页面

使用场景：有些时候样式在桌面chrome模拟是好的，但是在部分webview或者真机上就有问题。

#### . 第一步：npm install -g weinre
#### . 第二步：weinre –boundHost xx ip
#### . 类似于weinre –boundHost 172.16.28.162
#### . 第三步：此时weinre会返回一个可用的地址

````javscript

2015-12-14T03:58:50.349Z weinre: starting server at http://10.1.2.77:8080

````
也可以指定端口号
#### `weinre --httpPort 8081 --boundHost 172.16.28.162`
#### . 第四步：可以访问之前weinre分配过来的地址，进入后在页面最上部分可以看到 `Access Points` 下的 `debug client user interface http://10.1.2.77:8080/client/#anonymous`


![效果](http://cdn1.showjoy.com/images/9a/9a259c3bbf5d438399842c11d53054c9.png)

这是之后要点击的链接 ，我们先往当前网页下方看。可以看到 `Target Script`栏目



![效果](http://cdn1.showjoy.com/images/ad/add8ce371e0e4f52898fe4f652427f50.png)

我们需要将`Example`的`script`标签复制粘贴到需要调试的项目中去。
#### . 第五步：开启本地静态服务器（XMAPP等），进入到需要调试的项目的页面，例如`http://10.1.2.77:8000/index.html`我是利用`nodejs`开的一个小型服务器。
![XI](http://cdn1.showjoy.com/images/d5/d53d31ba1bcd41749446ae158e923ed8.png)
#### . 第六步：此时可以回到第四步中的weine分配给你的地址，
![SA](http://cdn1.showjoy.com/images/42/42eadf2d9af545629e14387a9c6c64bf.png)
点击第一条`debug client user interface: http://10.1.2.77:8080/client/#anonymous`,进入预备调试页面。
#### . 第七步：在预备调试页面我们可以看到
![asd](http://cdn1.showjoy.com/images/ca/ca2a26ecf08d4eca807ea1ec2374eee9.png)
`Targets`下有一条可以选择的调试页面，对应着刚刚在本地静态服务器中打开的页面。但是这个页面是PC的，现在我们通过手机（浏览器、APP、微信等）进入这个项目ip地址`http://10.1.2.77:8000/index.html`
此时预备调试页面将会出现两条可调试地址
![as](http://cdn1.showjoy.com/images/42/426b2c9ab5504cc18c77b1da504e7270.png)
选择手机端进入后新增的地址。
#### .第八步：点开链接后链接状态从蓝色变成绿色，然后再点击页面左上角的Element按钮。
![a](http://cdn1.showjoy.com/images/ae/ae11cb7dc7034bc4981503af603c6300.png)
现在就可以在pc端调试，手机端直接显示修改的变化啦。

#### 优点：为何称weinre为万金油呢？因为只要在需要调试的项目的文件中引入weinre给出的<scrip 标签，再配合本地静态服务器的ip。可以在任何ios或安卓机型的包括浏览器和app的webview中去调试。

#### 缺点： weinre不支持js的断点调试，这是一个缺憾，并且要在项目中引入额外的调试js标签。
但是对于js断点，可以在js逻辑中埋入console，然后在手机端真实操作，再在pc端的调试器中查看打印信息这种方式来代替。

> 3、MIHTool
#### 调试chrome safari浏览器的H5页面
注意关闭翻墙代理噢。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基于weinre 开发的一款APP， 只针对ios手机。大概使用方式是在APP内打开需要调试的页面，相应的在pc端浏览器会出现调试器，方可进行调试。
使用MIHTool的最大优点之一在于不需要显式的引入调试所需的脚本。在此基础上，作者还增加了一些方便的功能。

#### 1. Performance API.
#### 2. Polyfill管理器(模拟javascript与Native App互相调用,demo)
#### 3. NPM Modules (在 web inspector console 里通过 require() 加载 npm 模块)

具体可以看官网：<a href="http://www.mihtool.com/">http://www.mihtool.com/</a>

> 4、ios模拟器 + mac safari

#### 调试safari浏览器的H5页面（可进行js断点调试）

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在没有真机的情况下，可以采用模拟机的方式去运行ios系统下的safari浏览器，然后在safari浏览器内输入想要调试的页面。当然模拟ios系统必须在Xcode这个开发软件下进行。一旦在Xcode里开启了模拟机，再进入模拟机里的safari，之后按照上边介绍过的真机 safari + mac safari介绍的方式进行操作便可。

![a](http://cdn1.showjoy.com/images/ab/ab12718e529e4f03b1e17091e592b694.png)

> 5、ios模拟器 + mac safari

#### 调试公司APP内的webview页面（可进行js断点调试）

参考 <a href="https://github.com/paulirish/iOS-WebView-App">https://github.com/paulirish/iOS-WebView-App</a>，因为不懂IOS开发，也不知道是如何进行的，但大概是在iOS项目内写一些方法抛出一个地址，然后在mac上的safari开发模式内识别出这个地址，然后就可以调试的。默认情况下，mac safari只能识别usb连接的ios设备上的safari浏览器上的H5页面。（上面介绍过）


===========================================
===========================================

### <span style="color:red">Android 设备</span>

> 1、Android chrome 真机 + pc chrome

#### 调试移动端浏览器的H5页面（可进行js断点调试）

在安卓4.4版本以下的安卓机子，手机上安装了chrome的情况下，并且打开需要调试的页面，可以打开usb开发者模式，在pc端chrome地址栏输入chrome://inspect便可找到相应的手机的相应打开wap页面。调试方法跟pc chrome几乎一致。

在第一次使用的时候点击inspect会弹出空白的调试页面，这个时候需要翻墙，再点inspect才会出现正常的调试页面。

#### 优点：可进行js断点调试。
#### 缺点：浏览器限制。

#### 调试公司APP内的webview页面（可进行js断点调试）

在安卓4.4版本以上的安卓机子，手机通过usb连接mac电脑后，手机打开usb开发者模式后，在电脑chrome的chrome://inspect页面里会找到除了手机浏览器打开的页面，还会找到App内打开的webview的页面。

![a](http://cdn1.showjoy.com/images/ce/ce98e4652b7f47c99612a3081e4e785f.png)

点开`inspect`就是一个调试界面。

进行webview页面的调试 需要在应用的代码内设置才能生效（根据<a href="https://github.com/yujiangshui/CN-Chrome-DevTools/blob/remote-debugging/md/Use-Tools/remote-debugging.md#%E8%B0%83%E8%AF%95-webviews">https://github.com/yujiangshui/CN-Chrome-DevTools/blob/remote-debugging/md/Use-Tools/remote-debugging.md#%E8%B0%83%E8%AF%95-webviews</a>）


> 2、Android 模拟机 + pc chrome

&nbsp;&nbsp;&nbsp;&nbsp;安卓模拟机配置成功的情况下调试的方式跟上面1方法是一样的，在安卓4.4版本以上才能进行webview的调试。
&nbsp;&nbsp;&nbsp;&nbsp;下面就介绍下大名鼎鼎的安卓模拟器genymotion.个人使用是免费的。

1. 在其官网注册（必须）https://www.genymotion.com/
2. 在其官网下载最新的软件，下载完将genymotion和genymotion shell都放到应用程序内。
3. 下完这个之后还需要下载virtualBox，https://www.virtualbox.org/wiki/Downloads
4. 之后就可以选择不同版本的模拟器了，网上的教程都是推荐下载安卓4.3版本，因为4.3版本有对应的补丁，装上后可以避免一些应用下载失败。但4.3版本不支持调试webview。我推荐下载 HTC one 4.4.4版本，本人试了很多版本，只有这个可以调试我司App内嵌的H5页面。

![a](http://cdn1.showjoy.com/images/b3/b38d848454e34984a0436989c4847139.png)

推荐一个专门讲解genymotion的资源。
<a href="http://www.ziliao1.com/Article/Show/DCDFC03FE4902A1AC353C74695DAC2E9.html">http://www.ziliao1.com/Article/Show/DCDFC03FE4902A1AC353C74695DAC2E9.html</a>
以及模拟机版本下载失败的解决办法
<a href="http://www.cnblogs.com/wliangde/p/3678649.html">http://www.cnblogs.com/wliangde/p/3678649.html</a>

> 3、万金油 weinre + pc chrome

#### 调试任意浏览器的H5页面以及webview页面

这个方法在之上iOS 部分已经讲解过了。weinre通吃安卓与ios，兼容性不要太高。

> 4、微信调试

#### 调试微信客户端的webview页面（支持js断点调试）

目前微信已经出了官方的调试工具，照着做就好了。
<a href="http://blog.qqbrowser.cc/">http://blog.qqbrowser.cc/</a>

> 5、UC浏览器 + 真机 + mac chrome

#### 调试uc浏览器的H5页面（支持js断点）

方法一是真机加上chrome浏览器，这个方法是真机加uc浏览器，
<a href="http://www.uc.cn/business/developer/">http://www.uc.cn/business/developer/</a>


++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++

<span style="color:red">总结</span>

#### 上面罗列的多种方法，在本地开发的情况下基本都已经覆盖到了。但是如果线上页面出现bug了。该怎么办？最快的方法直接对线上页面进行调试。
先介绍一下mac抓包工具`charles`

<a href="http://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=400497314&idx=1&sn=850fad741fdb635c2403bafb2f1e636f&scene=1&srcid=1119TqsnfhBHSKzA2BUja9mk&key=ac89cba618d2d97693fea624e810ff69a7385f5209ab2a26ec029ca8f618e760e87de444299655bad41c5d1a7dbbc76e&ascene=0&uin=Mjg1NTY1OTY2MA%3D%3D&devicetype=iMac+MacBookPro12%2C1+OSX+OSX+10.10.4+build(14E46">charles 教程</a>


1、截取http网络封包
2、支持修改网络请求参数
3、支持网络请求的截获并动态修改
4、支持重发网络请求
5、模拟网速网络

刚刚给出的链接中有charles的全方位解读，我这里就简单介绍下http网络封包。

在pc上charles可以很容易的创建一个代理，将设定的一部分网络请求全部转到charles代理上，并且可以将其中某个资源map到本地，从而实现使用本地代码调试线上代码的功能。

但是我们现在是调试手机等移动端设备，但稍微做一些设置，就可以实现通过pc上的charles去代理移动端去请求资源。然后将需要的资源map到本地，进行修改和调试。

这样设置过后的效果就是，在pc端修改代码，移动端线上的页面去请求本地修改后的代码并展示。

讲完了charles,问题回到如何快速调试线上页面。

#### 如果是H5页面
遇上H5页面在PCchrome调试器上跟真实手机上的效果不一致时，可以使用万金油weinre，结合charles，就可以调试这种机型各种浏览器上的问题了。当然也可以使用上面提到的其他方法，都是适用的。

#### 如果是webview
同样不能少的是charles抓包，如果是安卓可以配合genymotion模拟器进行模拟。使用方法上文讲述过，再推荐一篇专门的文章：http://div.io/topic/1449。这篇文章讲到如何用charles监听genymotion。
如果是ios，也可以采用这样的方式，即模拟器+charles+mac safari。使用方法在上文的ios章节讲述过。
当然也是可以使用万金油weinre。weinre+charles调试App内的H5页面也是很方便的。跟上面两种方法比较，缺少的就是js断点调试功能。




===========================================


#### 声明:本文转载自<a href="http://lvdada.org/2015/12/20/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E8%B0%83%E8%AF%95/">lvdada的博客</a>

#### 完！

#### 借鉴+原创文章，转载请注明出处！
