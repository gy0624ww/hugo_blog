---
title: "Final Cut Pro 剪辑入门"
date: 2020-01-28T10:50:19+08:00
categories:
- 剪辑
tags:
- Final Cut Pro
- 剪辑
disqusIdentifier: "Final Cut Pro 剪辑入门"  
comments: true  
draft: false

keywords:
- Final Cut Pro
- 剪辑
thumbnailImage: https://s2.ax1x.com/2020/01/28/1MnnCq.md.jpg
thumbnailImagePosition: right
metaAlignment: center
coverImage: https://s2.ax1x.com/2020/01/28/1MQSeS.md.png
coverCaption: "大家都说来日方长，可这世间却世事难料"
coverMeta: in
coverSize: partial
summary: "剪辑视频利器-Final Cut Pro 入门操作"
---

<!--more-->
<!--toc-->

# 软件介绍

剪辑出一部好的片子缺少不了好用的工具，市面上电脑端常用有Adobe公司出品的`Premiere`（简称Pr），另外就是苹果公司出品的`Final Cut Pro`（简称FCP）了，其实这两款软件都很不错，也各有优缺点，我的电脑是Macbook,对`FCP`契合度更高一些，本文就重点介绍`FCP`为主,使用的是`Final Cut Pro 10.4.3`版本.

在移动市场覆盖面很广的背景下，一些移动端的剪辑软件也展露出随拍随剪的优势来，比如头条出品的`剪映`,和360出品的`快剪辑`等等。但是我觉得术业有专攻，简单的片子手机还可以驾驭的了，但是如果真的要做出一个做工精良的片子还是得在PC端上来解决，毕竟有强大的硬件支撑，而且随着拍摄工具的多样化:手机，微单，运动相机，无人机等等,从越来越多的地方拷出资源，显然在电脑里管理还是很方便的。

好了，咱言归正传

# 界面

`FCP`的操作界面分为四个部分，分别是：`浏览器`，`检视器`，`检查器`和`时间线`  
大家现有个大体的概念，  
浏览器顾名思义就是浏览各种素材的地方  
检视器就是预览的地方  
检查器就是调整各种参数的地方  
时间线就是排列组合各种素材和音频的地方

下面我们来详细看看各部分

## 浏览器

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/29/1MhuHP.png" src="https://s2.ax1x.com/2020/01/29/1MhuHP.png" group="group:none" thumbnail-width="" thumbnail-height="" title="浏览器" >}}

浏览器里包括`资源库`，`音频`和`字幕`相关的素材

## 检查器

这里可以查看一些视频的信息，比如视频的尺寸和分辨率，时长，编码器等等，也可以修改视频的一些参数，比如不透明度，缩放，旋转，曝光，调色等等。

## 时间线 

这里就是时间线区域：  

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/30/11wT10.png" src="https://s2.ax1x.com/2020/01/30/11wT10.png" group="group:none" thumbnail-width="" thumbnail-height="" title="时间线" >}}

这里就是组合各个素材的地方。想要什么就往里拖就可以了。

## 检视器

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/30/11whkj.png" src="https://s2.ax1x.com/2020/01/30/11whkj.png" group="group:none" thumbnail-width="" thumbnail-height="" title="检视器" >}}

在下面的时间线中按`空格键`开始播放,除了预览和播放，他还有别的功能：  

- 缩放，裁剪素材
- 调整播放速度  
- 调整颜色等等 

# 导入素材

## 资源库

上面说到浏览器是浏览各种素材的地方，当然我们最关心的就是管理本地拍摄的素材,资源库的层级结构依此为`资源库`->`事件`->`项目`  
资源库相当于一个总文件夹,可以包含多个事件，当新建一个资源库的时候，系统会默认给你创建一个以日期命名的事件。所以事件可以理解为资源库的二级文件夹，而项目依托于事件，必须在一个事件下建立项目。  

新建项目的时候选择对应的参数，需要注意的是，帧率最高支持60，如果选择了60，那么对将来导入的所有素材的要求就是必须都得大于等于60，所以一般来说选30就够用了。渲染编码就选择`Apple ProRes422`。  

建好项目之后，下方就会出现了一个空白的时间线，换句话说新建项目就是新建一条时间线，只要有时间线我们才能往里面添加素材。  



终于到了导入素材的时候了，大概有两种方法，首先是比较标准的做法：
选中一个事件，在空白处右键选择`新建媒体`，选择你要导入的视频素材，这里要注意的是右侧的文件拷贝选项：  

- 拷贝到资源库
- 让文件保留原位  

前者进行了一次复制，不是在源文件上修改，好处就是容错率高，缺点就是占用额外的空间，后者就是在源文件上进行修改: 

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/29/1QjrUe.png" src="https://s2.ax1x.com/2020/01/29/1QjrUe.png" group="group:none" thumbnail-width="" thumbnail-height="" title="文件拷贝选项" >}} 

还有一个选项就是转码的选择，如果要剪辑4K这种高分辨率的视频，但是电脑性能又跟不上，可以选择勾上`创建优化媒体`或`创建代理媒体`，这样能提高剪辑流畅性  

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/30/1Qz9j1.png" src="https://s2.ax1x.com/2020/01/30/1Qz9j1.png" group="group:none" thumbnail-width="" thumbnail-height="" title="转码" >}}

另外一种就是直接从`访达`里拖，简单粗暴。  

{{< alert info >}}
但是如果素材时间很长，那么把整个素材都拖进时间线太不方便了，比如有个30分钟的素材，我只要其中的2分钟，显然我把30分钟的素材都拖到时间线上不妥，那么我们最好只拖我们想要的部分，用快捷键在开始的地方按下`i`和结束的地方按下`o`
{{< /alert >}}

>i: Initiate 开始  
>o: Over 结束

选取后会有黄色的区域，直接拖该区域到时间线就OK了

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/30/11BOY9.md.png" src="https://s2.ax1x.com/2020/01/30/11BOY9.png" group="group:none" thumbnail-width="" thumbnail-height="" title="选取区域" >}} 

## 其他导入方式快捷键

- `E`: 把所选素材直接追加在时间线末尾，不覆盖其他片段    
- `Q`: 把所选素材放到当前时间轴的位置，不覆盖其他片段  
- `W`: 把所选素材放到当前时间轴的位置，时间轴所在的素材会自动切割出来在两边  
- `D`: 把所选素材放到当前时间轴的位置，时间轴所在的素材会自动切割开来，并被覆盖   

如果你记不住这几个快捷键，那么界面也有对应的按钮，就是下面这四个：

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/11sgoD.md.png" src="https://s2.ax1x.com/2020/01/31/11sgoD.png" group="group:none" thumbnail-width="" thumbnail-height="" title="快捷键" >}} 

那么如果我只想把素材的音频添加到时间线上怎么办呢，上面的四个按钮旁边有个下拉菜单，选择仅音频就OK了。当然也可以仅视频和全部。  

# 渲染 

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/11yhAU.md.png" src="https://s2.ax1x.com/2020/01/31/11yhAU.png" group="group:none" thumbnail-width="" thumbnail-height="" title="渲染进度" >}} 

看到这些小点了吗，每当时间线有更新软件就会在后台进行渲染，这些白点代表后台渲染的进度，当这些点没有了，就代表渲染结束了，时间线预览视频的时候会流畅很多，但是如果你的电脑配置不是很高，因为渲染同时占用系统资源，就有可能出现卡顿，影响剪辑的流畅性，那么你可以在`偏好设置`->`播放`里调整渲染延迟时间，或者直接关闭后台渲染。  

# 剪辑工具


{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/13ENYn.md.png" src="https://s2.ax1x.com/2020/01/31/13ENYn.png" group="group:none" thumbnail-width="" thumbnail-height="" title="剪辑工具" >}} 

这里使用频率最高的就是选择`A` 和切割`B`，切割可以在按时间线切割视频片段，选择片段可以移动片段位置。  

- 修剪（T） 可以左右拖动素材，当一头边界是红色的就说明拖到头了  
- 位置（P） 可以拖动素材片段，如果素材直接有中间空白则自动添加黑场
- 范围选择(R) 可以在素材上选择一块范围，比如想调整某一段的音量就可以这么圈选
- 缩放（Z） 可以缩放时间线，不常用，一般都直接用触控板来缩放，更方便


{{< alert info >}}
如果要在界面上显示所有时间线的内容，用快捷键`shirt+Z`。就会自动缩放所有时间线到合适的大小以全部显示
{{< /alert >}}
 
## 如何添加转场 

打开转场素材，查看目前系统已经安装的转场素材：

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/13s4vn.md.png" src="https://s2.ax1x.com/2020/01/31/13s4vn.png" group="group:none" thumbnail-width="" thumbnail-height="" title="转场" >}} 

然后把转场素材片段拖到两个视频片段之间就可以了。

## 如何添加效果 

打开效果素材列表：


{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/13ybLt.md.png" src="https://s2.ax1x.com/2020/01/31/13ybLt.png" group="group:none" thumbnail-width="" thumbnail-height="" title="效果" >}} 

和转场操作类似，把对应的效果拖到要生效的视频片段上就行了。

## 音乐淡入淡出

音乐淡入淡出也是比较常用的操作。来控制音乐结束不那么突兀。

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/136TtU.md.png" src="https://s2.ax1x.com/2020/01/31/136TtU.png" group="group:none" thumbnail-width="" thumbnail-height="" title="声音淡入淡出" >}} 

具体做法就是拖动音频部分的两个小按钮，当出现左右的剪头`<- ->`的时候 向左或右拖动，就会看到有个曲线生成，曲线越平声音越小。


{{< alert info >}}
默认是视频和音频是在一起的，单独调整视频容易误操作，最好是音频和视频分离开，右键片段选择分离音频就可以把音频和视频分成两个并行的片段。
{{< /alert >}}

## 如何添加字幕 

打开浏览器里的字幕选项卡，选择字幕特效，然后拖到对应添加字幕的视频片段上，再在检查器区域配置字幕相关属性或者直接在监视器上填写字幕内容

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/132VFf.md.png" src="https://s2.ax1x.com/2020/01/31/132VFf.png" group="group:none" thumbnail-width="" thumbnail-height="" title="添加字幕" >}} 

# 保存工作区

上面所提到的界面如果都展开的话有些人会觉得很杂乱，可以通过右上角的三个按钮来显示或隐藏部分内容来精简操作环境  

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/13hvxU.md.png" src="https://s2.ax1x.com/2020/01/31/13hvxU.png" group="group:none" thumbnail-width="" thumbnail-height="" title="保存工作区" >}} 

之后可以对比较常用的工作区组合进行保存，可以在`窗口`->`工作区`->`将工作区保存为`进行保存

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/1341it.md.png" src="https://s2.ax1x.com/2020/01/31/1341it.png" group="group:none" thumbnail-width="" thumbnail-height="" title="保存工作区" >}} 

以后就可以在特定的流程中快速的进行切换，比如在到了调色环境，就可以切换到之前搭配好的调色界面窗口组合

# 导出

右上角的分享按钮，选择`母版文件`。然后选择设置选项卡：


{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/13Tq3t.md.png" src="https://s2.ax1x.com/2020/01/31/13Tq3t.png" group="group:none" thumbnail-width="" thumbnail-height="" title="导出" >}} 

然后在格式选择`电脑`，视频编码选择`H.264较好质量` 点击下一步，就会在后台进行渲染并导出了。  

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/137sr8.md.png" src="https://s2.ax1x.com/2020/01/31/137sr8.png" group="group:none" thumbnail-width="" thumbnail-height="" title="导出" >}} 


在左上角可以查看渲染进度 

{{< image classes="fancybox center clear" thumbnail="https://s2.ax1x.com/2020/01/31/137JbD.md.png" src="https://s2.ax1x.com/2020/01/31/137JbD.png" group="group:none" thumbnail-width="" thumbnail-height="" title="导出" >}} 


以上就是`Final Cut Pro X`的常用的入门操作。以后拍出好片的时候我再结合实际例子实操一下  

{{< hl-text danger >}}
敬请期待😃  
{{< /hl-text >}}