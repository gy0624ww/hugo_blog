---
title: "OpenTracing的介绍与实践(二)"
date: 2021-04-09 14:09:09+08:00
draft: false
categories:
- 后端
- Go
tags:
- golang
- 微服务
disqusIdentifier: "OpenTracing的介绍与实践(二)"
comments: true

keywords:
- opentracing
- 链路追踪
- 微服务
- OpenTracing的介绍与实践
- OpenTracing的最佳实践

thumbnailImage: https://miro.medium.com/fit/c/200/200/1*drzMINEw0ObG5HeBX7OLDw.png
thumbnailImagePosition: right
metaAlignment: center
coverImage: https://s1.ax1x.com/2020/06/29/NWulT0.md.jpg
coverCaption: "有些人，再见一面才能忘记"
coverMeta: in
coverSize: partial
summary: "OpenTraacing入门实践-Lesson 1"
---
<!--more-->
<!--toc-->  

{{< alert warning >}}
本文主体内容译自官方教学并可能稍加修改，原版 [传送门](https://github.com/yurishkuro/opentracing-tutorial/tree/master/go)
{{< /alert >}}

# 本课程目标

你将学会：  

- 初始化一个Tracer
- 创建一个简单的trace
- 丰富trace相关信息

# 演示
## A simple Hello-World program

我们首先来创建一个简单的应用：`lesson01/hello.go`, 大概逻辑是接收一个参数并且用程序打印出来：  

{{< tabbed-codeblock lesson01 "https://github.com/yurishkuro/opentracing-tutorial/blob/master/go/lesson01/solution/hello.go" >}}
<!-- tab go -->
package main
import (
    "fmt"
    "os"
)

func main() {
    if len(os.Args) != 2 {
        panic("ERROR: Expecting one argument")
    }
    helloTo := os.Args[1]
    helloStr := fmt.Sprintf("Hello, %s!", helloTo)
    println(helloStr)
}
<!-- endtab -->
{{< /tabbed-codeblock >}}

运行结果：

    $ go run ./lesson01/hello.go Bryan
    Hello, Bryan!  
    
## 创建一个trace

上文已经说过，trace 是由span组成的有向无环图，`span`是链路记录逻辑的标记。每个span至少含有如下属性：  

- 一个操作的名字（operation name）
- 一个开始时间(start time)
- 一个结束时间(finish name)  

下面我们来创建只含有一个span的trace。首先我们需要一个`opentracing.Tracer`的实例。我们可以用`opentracing.GlobalTracer()`返回的全局实例先来代替

{{< tabbed-codeblock lesson01 "https://github.com/yurishkuro/opentracing-tutorial/blob/master/go/lesson01/solution/hello.go">}}
<!-- tab go -->
...
...
helloStr := fmt.Sprintf("Hello, %s!", helloTo)

tracer := opentracing.GlobalTracer()

span := tracer.StartSpan("say-hello")
println(helloStr)
span.Finish()
<!-- endtab -->
{{< /tabbed-codeblock >}}



上面代码我们展示了 OpenTracing API的一些基本功能和特性：

- 一个`tracer`实例是用来创建一个新的span,通过 `StartSpan` 方法
- 每个`span` 要输入一个操作名，上面代码中输入的是`say-hello`
- 每个`span` 记录完信息后必须调用他的`Finish()`方法
- tracer会自动的记录开始和结束时间戳

## 初始化一个真正的tracer

我们基于jaeger来初始化一个tracer的实例，参考[(http://github.com/uber/jaeger-client-go)](http://github.com/uber/jaeger-client-go)  

{{< tabbed-codeblock lesson01 "https://github.com/yurishkuro/opentracing-tutorial/blob/master/go/lesson01/solution/hello.go"  >}}

<!-- tab go -->
import (
	"fmt"
	"io"

	opentracing "github.com/opentracing/opentracing-go"
	jaeger "github.com/uber/jaeger-client-go"
	config "github.com/uber/jaeger-client-go/config"
)

// initJaeger returns an instance of Jaeger Tracer that samples 100% of traces and logs all spans to stdout.
func initJaeger(service string) (opentracing.Tracer, io.Closer) {
    cfg := &config.Configuration{
        ServiceName: service,
        Sampler: &config.SamplerConfig{
            Type:  "const",
            Param: 1,
        },
        Reporter: &config.ReporterConfig{
            LogSpans: true,
        },
    }
    tracer, closer, err := cfg.NewTracer(config.Logger(jaeger.StdLogger))
    if err != nil {
        panic(fmt.Sprintf("ERROR: cannot init Jaeger: %v\n", err))
    }
    return tracer, closer
}
<!-- endtab -->
{{< /tabbed-codeblock >}}

我们在main函数里采用这个来初始化tracer:

{{< tabbed-codeblock lesson01 "https://github.com/yurishkuro/opentracing-tutorial/blob/master/go/lesson01/solution/hello.go" >}}
<!-- tab go -->
...
...
tracer, closer := initJaeger("hello-world")
defer closer.Close()
...
...
<!-- endtab -->
{{< /tabbed-codeblock >}}

我们可以看到我们传递了一个参数`hello-world`到初始化方法里，用来标记是在一个叫`hello-service`的服务下的tracer创建的span。  

现在试下执行下程序，我们会看到下面的日志：

    $ go run ./lesson01/hello.go Bryan
    2017/09/22 20:26:49 Initializing logging reporter
    Hello, Bryan!
    2017/09/22 20:26:49 Reporting span 5642914c078ef2f0:5642914c078ef2f0:0:1
    

如果你已经运行的jaeger,你就可以在UI里查询到这个trace。  

## 丰富trace相关信息，Tags & Logs

现在我们创建的trace还是很基础的。如果我们的程序执行用`hello.go Susan` 替代 `hello.go Bryan`， 那么两个trace结果会相当难以区分。  
如果我们在trace能够捕获到参数来区分他们就会非常nice。  

一种简单的方法就是实用字符串`Hello,Bryan!` 替代`say-hello`作为span的操作名。然而这种做法在分布式链路追踪是非常不推荐的，因为操作名是代表一组span,而不是独立的一个实例。  
举个栗子，在`Jaeger UI`里你可以下拉选择里选择操作名来查找筛选traces,那将会是非常糟糕的用户体验，如果我们运行了1000次程序，对1000个人说hello,那么下拉框就得有1000个条目，  

另一个选择更统称一些名字作为操作名的原因是这样更方便聚合。再举个栗子，`Jaeger tracer`有一个选项是对应用流量生成指标，如果用不同的相互独立的操作名那么这统计将毫无用处。  

推荐的解决方案是为用tags,logs为span打上批注来丰富span的内容  

`tag` 是一个k/v键值对，提供了和span有关的一些元数据  
`log` 是类似于普通的日志语句，包含了一个时间戳和一些数据，只不过是依附于span里的  

那我们该什么时候用tags和logs呢？  
tags用于描述应用于整个span的属性。例如如果一个span记录的是一个HTTP请求，那么请求的URL就应该被标记成tag,
因为没有道理认为URL只在span的不同时间点才相关。另一方面，如果服务器响应返回一个重定向URL，那么应该用log记录他比较合理，因为有一个明确的时间点去记录这个响应事件。  
OpenTracing提供了相关的[指南](https://github.com/opentracing/specification/blob/master/semantic_conventions.md)来提供推荐的tags和logs参数。  

{{< alert  info >}}
*笔者注：span tag属性适用于能代表整个span生命周期的属性，整段期间内都适用，而不是仅仅其中某个时间点才适用。比如记录span时用的一些常量配置等信息。  
而Log是记录的span周期内的某一个时间点发生的事件*
{{< /alert >}}

### Using Tags

在`hello.go Bryan`的例子中，其中`Bryan`是一个很好的span tag选项，因为他适用于整个span周期，并且不仅是存在某一时间点，我们可以这样记录他：  

{{< tabbed-codeblock lesson01 "https://github.com/yurishkuro/opentracing-tutorial/blob/master/go/lesson01/solution/hello.go" >}}
<!-- tab go -->
...
...
span := tracer.StartSpan("say-hello")
span.SetTag("hello-to", helloTo)
...
...
<!-- endtab -->
{{< /tabbed-codeblock >}}


### Using Logs

由于我们现在的程序还太过于简单，以至于不好找明显的记录Log的例子，但是我们还是来试一试。我们现在马上格式化输出`helloStr`并且打印输出，这两项操作都有特定的时间点，所以我们可以在他们执行完来记录完成情况。  

{{< tabbed-codeblock lesson01 "https://github.com/yurishkuro/opentracing-tutorial/blob/master/go/lesson01/solution/hello.go" >}}
<!-- tab go -->
...
...
helloStr := fmt.Sprintf("Hello, %s!", helloTo)
span.LogFields(
    log.String("event", "string-format"),
    log.String("value", helloStr),
)

println(helloStr)
span.LogKV("event", "println")

<!-- endtab -->
{{< /tabbed-codeblock >}}

如果你对结构化日志API不熟悉，那么这个log语法可能看起来有些奇怪，与将日志消息格式化成便于阅读的字符串不同，结构化日志API更鼓励你把日志信息拆分成若干个k/v键值对来记录，  
这样方便日志聚合系统收集加工。这种做法主要是考虑到如今的日志是主要是程序来读的而不是人类。更多结构化日志信息请Google `structured-logging`  

OpenTracing API 为Go提供了两种风格的记录log的方式：  

- `LogFields` 方法，提供了强类型声明 k/v键值对
- `LogKV` 方法，提供了可变参数列表交替式键值对`key1,value1,key2,value2`（使用更简单）  

OpenTracing规范还建议所有日志语句包含一个`event`字段，用来记录整个事件，并提供该事件的其他属性值作为附加字段。


{{< alert success no-icon >}}
(未完)Continue...
{{< /alert >}}




# 参考文献

- [opentracing-tutorial](https://github.com/yurishkuro/opentracing-tutorial/tree/master/go)


