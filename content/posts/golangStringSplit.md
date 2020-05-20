---
title: Golang 使用 string.split 切割字符串并返回数组
date: 2019-09-11
author: "me"
tags: ["Golang", "原创"]
categories: ["后端"]
---

## Why

在很多业务需求或数据处理中，我们需要将某一字符串根据某分隔符一一隔开并置于数组中，像是已知 Id 字符串 `3,7,123` ，需要将 3、7、123 存入数组。在 golang 中，我们如何对此进行处理？下文将对此问题进行解答。

## What

在 Golang 中，我们可通过 `strings` 包的 `Split` 进行切割，并返回相应的数组，基础示例如下：

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    s := strings.Split("a,b,c", ",")
    // s := strings.Split("a b c", " ")
    // s := strings.Split("a|b|c", "|")
    // s := strings.Split("", ",")
    fmt.Println(s)
}
```

以上示例输出为 `[a b c]`

## How

上述示例已经可满足基本使用（注掉的代码可测试不同分隔符效果，以及空数组返回），该函数定义如下：

```golang
func Split(s, sep string) []string        
// s   - 需要分割的字符串
// sep - 分隔符
```

更多，可参考官方文档：https://golang.org/pkg/strings/#Split

另外，提供了同类的分隔函数以适应不同的业务需要，如下：

- func [SplitN](https://golang.org/pkg/strings/#SplitN) 加入返回数量限制
- func [SplitAfter](https://golang.org/src/strings/strings.go?#SplitAfter) 包含分隔符
- func [SplitAfterN](https://golang.org/src/strings/strings.go?#SplitAfterN) 包含分隔符 & 加入返回数量限制

```golang
The count determines the number of substrings to return:
n > 0: at most n substrings; the last substring will be the unsplit remainder.
n == 0: the result is nil (zero substrings)
n < 0: all substrings
```


