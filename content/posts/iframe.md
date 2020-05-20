---
title: 如何在页面中优雅地内嵌 iframe ？
date: 2019-09-09
author: "me"
tags: ["HTML", "iframe", "原创"]
categories: ["前端"]
---

## Why

在一些业务中，我们可能需要用到 iframe 来实现一些特定的需求，而如何在页面中嵌入 iframe 页面？通过本文，我们将稍加了解，至于为何强调了优雅？主要由于内嵌页面可能会在展示上有点突兀。对此，本文也进行了一些尝试，如去边框等。

## What

**iframe ？** **HTML内联框架元素()** 表示嵌套的 [browsing context](https://developer.mozilla.org/en-US/docs/Glossary/browsing_context)。它能够将另一个HTML页面嵌入到当前页面中。

具体，可参考：https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe

## How

### 基本使用

使用 `iframe` 标签，可将另一 HTML页面内嵌于当前页面中，基本使用示例如下：

```html
<iframe src="./main.html"></iframe>

<iframe id="mainIframe" name="mainIframe" src="http://www.baidu.com" frameborder="0" scrolling="auto" ></iframe>
```

以下列举了一些 iframe 常用属性：
| 属性 | 值 | 描述 |
| -------------- | ------------- | ------------------------------------------ |
| src | *URL* | 定义 iframe 中显示的文档的 URL |
| width | *pixels* *%* | 定义 iframe 的宽度 |
| height | *pixels* *%* | 定义 iframe 的高度 |
| name | *frame_name* | 定义 iframe 的名称 |
| frameborder |1 0 | 规定是否显示框架周围的边框 |
| marginheight |*pixels* | 定义 iframe 的顶部和底部的边距。 |
| marginwidth |*pixels* | 定义 iframe 的左侧和右侧的边距 |
| scrolling |yes no auto | 规定是否在 iframe 中显示滚动条 |
| srcdoc (HTML5) | *HTML_code* | 定义在 <iframe> 中显示的页面的 HTML 内容。 |
| sandbox (HTML5) | "" allow-form sallow-same-origin allow-scripts allow-top-navigation | 启用一系列对 <iframe> 中内容的额外限制 |

### 显示处理

通常我们需要对内嵌的 iframe 页面做一些样式上的调整，如下例：

![iframe_ex1](http://image.ftopia.cn/blog/iframe_ex1.png)

借用 [w3school](https://www.w3school.com.cn/tiy/t.asp?f=html_iframe) 代码沙箱，我们尝试内嵌 wap.baidu 页面，调整了了高度、宽度，并将边框去除

![iframe_ex3](http://image.ftopia.cn/blog/iframe_ex3.png)

处理过后，基本上和我们的页面融为一体了，如果不查看 html 源码，并不会注意到是通过 iframe 内嵌的。

### 其他问题

#### 跨域

iframe 间可能存在跨域问题，如何实现 iframe 间通信以及一些相关问题？

这个问题与本文主要讨论的有点偏离，可参考 [iframe跨域的几种常用方法](https://juejin.im/post/5cc4fc445188252e83434c32) 加以了解。

### 参考

更多 iframe 相关内容，可参考：

- [iframe 有什么好处，有什么坏处 ... From 知乎](https://www.zhihu.com/question/20653055)
- [iframe,我们来谈一谈 From 掘金](https://segmentfault.com/a/1190000004502619)


