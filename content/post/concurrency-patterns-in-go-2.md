---
title: "Concurrency Patterns in Go 2"
date: 2020-01-06T19:58:40+08:00
categories:
- category
- subcategory
tags:
- tag1
- tag2
keywords:
- tech
#thumbnailImage: //example.com/image.jpg
---

<!--more-->

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

