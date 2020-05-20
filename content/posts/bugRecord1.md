---
title: 记一个前端 moment.js 时区取错的 Bug
date: 2019-10-08
author: "me"
tags: ["bug记录", "moment.js", "时区", "原创"]
categories: ["前端"]
---

## What

国庆前一天，内部系统反馈一个前端Bug，选择本周的开始时间错误导致数据拉取为空。经测试可每次复现，奇怪的此页面发布后三个月来没发现且最近无相关变更。

![timebug_record](http://image.ftopia.cn/blog/timebug_record.png)

查了代码然后细想了下，原来是时区惹的祸，至于为什么那么久没发现？原因是通常内部系统周日没人用。。。

## How

问题找到，怎么解决就容易的多了。项目中，引入了 [moment.js](https://momentjs.com/docs/) 的时间处理类库，我们来看下更改的代码，如下：

```
-const thisMonday = moment().day(1).hour(0).minute(0).second(0).millisecond(0);
+const thisMonday = moment().isoWeekday(1).hour(0).minute(0).second(0).millisecond(0);
```

熟悉的应该就明白了，[day()](https://momentjs.com/docs/#/get-set/day/) 方法默认取 Sunday 为 0，自然周日就比周一的时间要早了。

> This method can be used to set the day of the week, with Sunday as 0 and Saturday as 6.

moment.js 提供了解决此类问题的方法，即使用 [isoWeekday()](https://momentjs.com/docs/#/get-set/iso-weekday/) 方法。

**后记**：Bug 虽小，但时间尤其是时区处理是需要特别注意的，前后端都要哦！