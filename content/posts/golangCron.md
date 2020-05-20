---
title: Golang 实现定时任务
date: 2019-08-29
author: "me"
tags: ["Golang", "定时", "原创"]
categories: ["后端"]
---

## Why
在应用开发中，经常需要一些周期性的操作，如：在每天凌晨分析前一天的日志、每隔5分钟检查某些业务情况并触发告警等等。这些功能需要使用定时任务的方法去实现，那我们是如何使用 Golang 去实现的呢？



## What

推荐使用 github 上的 Golang 开源库来实现定时任务，介绍两个常用的库：robfig/cron 和 jasonlvhit/gocron

- [robfig/cron](https://github.com/robfig/cron)：说到定时任务，会想到 `crontab`，其常见于Unix和类Unix的操作系统之中。robfig/cron 库使用了类 crontab 的方式来执行定时任务。

- [jasonlvhit/gocron](https://github.com/jasonlvhit/gocron)：类 crontab 的设置方式可能并不友好，jasonlvhit/gocron 提供了更为人性化的执行方式。

至于选取哪个？熟悉 crontab 的可以选择 cron，想可读性佳的可以选择 gocron，两者的功能及稳定性都还不错。



## How

### cron
**安装** 	`go get -u github.com/robfig/cron`

**Demo**

```go
package main

import (
	"log"

	"github.com/robfig/cron"
)

func main() {
	i := 0
	c := cron.New()
	spec := "*/5 * * * *"
	c.AddFunc(spec, func() {
		i++
		log.Println("execute per 5 seconds", i)
	})
	c.Start()
	select {}
}
```
执行结果如下（代码说明：每5秒执行一次，i累加并记录打印）：
![cron demo](http://image.ftopia.cn/blog/golang_cron_1.png)

> 其中，select的用法：golang 的 select 的功能和 select, poll, epoll 相似，就是监听 IO 操作，当 IO 操作发生时，触发相应的动作。

类似的，如果需要执行每分钟执行，其中 `spec := "0 */1 * * * *"` 改一下即可，了解 crontab 的人应该不陌生其表达格式。总结如下：
| 字段名            | 是否必须 | 允许的值        | 允许的特定字符 |
| ----------------- | -------- | --------------- | -------------- |
| 秒(Seconds)       | 是       | 0-59            | * / , –        |
| 分(Minutes)       | 是       | 0-59            | * / , –        |
| 时(Hours)         | 是       | 0-23            | * / , –        |
| 日(Day of month)  | 是       | 1-31            | * / , – ?      |
| 月(Month)         | 是       | 1-12 or JAN-DEC | * / , –        |
| 星期(Day of week) | 是       | 0-6 or SUM-SAT  | * / , – ?      |

**备注：**
1. 月和星期段的值不区分大小写，如：SUN、Sun和 sun是一样的。
2. 星期(Day of week)字段以前的版本貌似非必填，默认* 现在版本就带上吧。

**特殊字符说明**
1. 星号( * )：表示 cron表达式能匹配该字段的所有值。
2. 斜线( / )：表示增长间隔；如：`spec := "*/5 * * * * *" `
3. 逗号( , )：用于枚举值，如第6个字段值是 MON,WED,FRI，表示星期一、三、五执行；又例如: `spec := "* 0,59 1 * * *"`，表示每天01:00和 01:59分的每秒都执行一次
4. 连字号( - )：表示一个范围，如第3个字段的值为 9-17 表示 9am到 5pm直接每个小时（包括9和17）
5. 问号( ? )：只用于日(Day of month)和星期(Day of week)，表示不指定值，可以用于代替 *

你也可以使用一些预设的定时器来方便执行，如下：

Entry                  | Description                                | Equivalent To
-----                  | -----------                                | -------------
@yearly (or @annually) | 每年执行 Run once a year, midnight, Jan. 1st        | 0 0 0 1 1 *
@monthly               | 每月执行 Run once a month, midnight, first of month | 0 0 0 1 * *
@weekly                | 每周执行 Run once a week, midnight between Sat/Sun  | 0 0 0 * * 0
@daily (or @midnight)  | 每天执行 Run once a day, midnight                   | 0 0 0 * * *
@hourly                | 每小时执行 Run once an hour, beginning of hour        | 0 0 * * * *

更多参考：https://godoc.org/github.com/robfig/cron



### gocron

**安装**  `go get -u github.com/jasonlvhit/gocron`

**Demo**

```go
package main

import (
	"log"

	"github.com/jasonlvhit/gocron"
)

func main() {
	i := 0
	s := gocron.NewScheduler()
	s.Every(5).Seconds().Do(func() {
		i++
		log.Println("execute per 5 seconds", i)
	})
	<-s.Start()
}

```
执行效果如下：
![gocron demo](http://image.ftopia.cn/blog/golang_gocron.png)

以上为基础定时器使用方式，gocron接口的命名及使用相对人性化，基本可直读，参考以下官方示例：
```go
package main

import (
	"fmt"
	"github.com/jasonlvhit/gocron"
)

func task() {
	fmt.Println("I am runnning task.")
}

func taskWithParams(a int, b string) {
	fmt.Println(a, b)
}

func main() {
	// Do jobs with params
	gocron.Every(1).Second().Do(taskWithParams, 1, "hello")
	
	// Do jobs safely, preventing an unexpected panic from bubbling up
	gocron.Every(1).Second().DoSafely(taskWithParams, 1, "hello")

	// Do jobs without params
	gocron.Every(1).Second().Do(task)
	gocron.Every(2).Seconds().Do(task)
	gocron.Every(1).Minute().Do(task)
	gocron.Every(2).Minutes().Do(task)
	gocron.Every(1).Hour().Do(task)
	gocron.Every(2).Hours().Do(task)
	gocron.Every(1).Day().Do(task)
	gocron.Every(2).Days().Do(task)

	// Do jobs on specific weekday
	gocron.Every(1).Monday().Do(task)
	gocron.Every(1).Thursday().Do(task)

	// function At() take a string like 'hour:min'
	gocron.Every(1).Day().At("10:30").Do(task)
	gocron.Every(1).Monday().At("18:30").Do(task)

	// remove, clear and next_run
	_, time := gocron.NextRun()
	fmt.Println(time)

	gocron.Remove(task)
	gocron.Clear()

	// function Start start all the pending jobs
	<- gocron.Start()

	// also, you can create a new scheduler
	// to run two schedulers concurrently
	s := gocron.NewScheduler()
	s.Every(3).Seconds().Do(task)
	<- s.Start()
}
```
更多参考：https://godoc.org/github.com/jasonlvhit/gocron
