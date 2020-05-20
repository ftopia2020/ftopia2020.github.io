---
title: Golang 使用 go-yaml 解析 YAML 文件
date: 2019-09-25
author: "me"
tags: ["Golang", "YAML", "原创"]
categories: ["后端"]
---

## Why

在项目中，难免会用到配置文件，而 YAML 格式的配置文件较 xml、json 等有其特有的优势，目前比较流行，那我们用 Golang 如何来解析 YAML 文件呢？



## What

### YAML

YAML 是 "YAML Ain't a Markup Language"（YAML不是一种置标语言）的递归缩写。YAML 以数据为中心，使用空白，缩进，分行组织数据，从而使得表示更加简洁易读。一个典型的 YAML 如下：

```yaml
title: YAML support for the Go language
category: Golang
tag:
- GoLang
- YAML
```

基本语法为：

- 使用缩进表示层级关系
- 禁止使用tab缩进，只能使用空格键
- 缩进长度没有限制，只要元素对齐就表示这些元素属于一个层级



### go-yaml

go-yaml 是支持 golang 解析 YAML 的三方库，github地址为： [https://github.com/go-yaml/yaml](https://link.zhihu.com/?target=https%3A//github.com/go-yaml/yaml) 



## HOW

学习之前，你可能需要了解：

- yaml 语言基础（上方仅简单介绍）
- Golang 基础使用、io/ioutil 标准库的基础了解

### 安装 go-yaml

```shell
$ go get gopkg.in/yaml.v2
```

### 解析 YAML

```go
package main

import (
	"fmt"
	"log"

	"gopkg.in/yaml.v2"
)

func main() {
	yamlContent := `title: YAML support for the Go language
category: Golang
tag:
- GoLang
- YAML`
	result := make(map[string]interface{})

	err := yaml.Unmarshal([]byte(yamlContent), &result)
	if err != nil {
		log.Fatalf("error: %v", err)
	}
	fmt.Println(result)
}

```

使用 `yaml.Unmarsha1`  来解析 yaml 源数据，并赋值 map；上述示例仅供学习理解

实际项目中，我们需要定义返回的结构体 struct，并且使用 `io/ioutil` 进行文件读取，改写参考如下：

![go_yaml_demo](http://image.ftopia.cn/blog/go_yaml_demo.png)

嵌套的 YAML 需要 struct 定义正确，解析基本就没问题，须注意数据类型！！

![go_yaml_demo2](http://image.ftopia.cn/blog/go_yaml_demo2.png)

以上示例解析了一个简单的嵌套。


