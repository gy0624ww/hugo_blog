---
title: "Go并发模式（一）"
date: 2019-11-18T10:50:19+08:00
categories:
- 后端
- Go 
tags:
- Go
- 并发模式
disqusIdentifier: Go并发模式（一）  
comments: true  
draft: true  

keywords:
- Go
- 并发模式
- 后端
thumbnailImage: https://s2.ax1x.com/2019/12/20/QOLqXj.jpg
thumbnailImagePosition: right
metaAlignment: center
coverImage: https://s2.ax1x.com/2019/12/20/QXE5s1.png
coverCaption: "A beautiful sunrise"
coverMeta: in
coverSize: partial
summary: "Go语言的最大的特性就是并发这一块，但是写好并且写出优雅的并发代码也是有挑战的一件事，我们今天开始来讲一些常见的并发模式,来打开Go语言新世界的大门！"
---

<!--more-->
<!--toc-->
# 前言  
  不知道大家有没有过，基础语法明明已经掌握了，但是一看别人的源码还是一头雾水，总有一种我和他学的不是同一门语言的赶脚，有没有看到别人的思路总是感觉那么新奇，写法那么的独特，困惑的同时又羡慕呢？

  其实这个和其他语言一样是存在设计模式的

{{< alert info >}}
  所谓设计模式, 就是大家经过一段时间实践并总结出来的一种解决问题的方法模式
{{< /alert >}}

  然而这个设计模式，就像武功秘籍一样，分不同的招式，来应对不同场景的问题，大家只要领会了招式的内涵，加上勤加苦练，相信大家也会写出一手优美的代码来的

下面我们来进入正题啦：  

# close-channel

我们知道当channel被关闭的时候，我们可以从中读取到:


{{< tabbed-codeblock "close-channel.go" "go" "go" >}}
<!-- tab go -->
intStream := make(chan int)
close(intStream)
integer, ok := <- intStream // 我们能从close channel 读取到值,ok=false,并且channel里类型的默认值
fmt.Printf("(%v):%v", ok, integer)
// output: (false):0
<!-- endtab -->
{{< /tabbed-codeblock >}}

我们并没有往channel里写入任何东西，当我们关闭channel之后我们依旧可以不断的从channel中读取，下面的一些模式里我们会经常用到这一特性

比如这样：

{{< tabbed-codeblock "close-channel.go" "go" "go" >}}
<!-- tab go -->
intStream := make(chan int)
go func() {
  defer close(intStream)
  for i:=1;i<=5;i++ {
    intStream <-i
  }
}

for integer := range intSteam {
  fmt.Printf("%v ", integer)
}
<!-- endtab -->
{{< /tabbed-codeblock >}}

上面的例子里我们确保我我们执行完goroutine之后关闭了`intStream`, 并且提供了循环的终止条件，这是非常常见的模式
  

{{< alert info >}}
关闭channel也是同时通知多个`goroutine`方法之一。
{{< /alert >}}

{{< tabbed-codeblock "close-channel.go" "go" "go" >}}
<!-- tab go -->
begin := make(chan interface{})
var wg sync.WaitGroup
for i:=0;i<5;i++ {
  wg.Add(1)
  go func(i int) {
    defer wg.Done()
    <-begin
    fmt.Printf("%v has begun\n", i)
  }(i)
}
fmt.Println("Unblocking goroutines...")
close(begin)
wg.Wait()
<!-- endtab -->
{{< /tabbed-codeblock >}}

# channel约束 
我们知道在在已关闭的channel写入会发生panic,

想必大家看见过很多次这种写法  
# for-select 

{{< tabbed-codeblock "for-select.go" >}}
<!-- tab go -->
for { // 无限循环或者range 某个范围
  select {
    // 这里可以配合channel做一些事情
  }
}
<!-- endtab -->
{{< /tabbed-codeblock >}}

没错，这是一个比较基础的组合，比如创建一个`goroutine`,并且在里面进行无限循环处理某项工作    
比如说有的时候我们想把一些可以进行循环迭代的数据拆出来塞进`channel`里，比如这样:

{{< tabbed-codeblock "for-select.go" >}}
<!-- tab go -->
for _,s := range []string{"one", "two", "three"} {
  select {
    case <-done: // 等待退出循环
    return
    case stringStream <-s: 
  }
}
<!-- endtab -->
{{< /tabbed-codeblock >}}

下面是这个的两个变种写法：

{{< tabbed-codeblock "for-select.go" >}}
<!-- tab style1 -->
for {
  select {
    case <-done: // 等待退出循环
    return
    default:
  }
  // 做一些非抢占式的工作
}
<!-- endtab -->

<!-- tab style2 -->
for {
  select {
    case <-done: // 等待退出循环
    return
    default:
      // 做一些非抢占式的工作
  }
}
<!-- endtab -->
{{< /tabbed-codeblock >}}


至于采用哪种写法取决于你的习惯了, 这个组合虽然很简单，但是要记好了，下面的其他并发模式会经常遇到

# 子不教父之过

在并发模式里有个不成文的约定：

{{< alert warning >}}
如果在一个goroutine里创建了一个子goroutine, 那父goroutine也必须得确保能随时关掉这个子goroutine
{{< /alert >}}

大家先记住这句话，我们先看一个例子：

{{< tabbed-codeblock "for-select.go" >}}
<!-- tab go -->

<!-- endtab -->
{{< /tabbed-codeblock >}}

这个约定会确保我们的程序可扩展性，下下面的的并发模式也会用到这种思想


# 本文参考

- [《Concurrency in GO》](https://raw.githubusercontent.com/KeKe-Li/book/master/Go/Concurrency in Go.pdf)

