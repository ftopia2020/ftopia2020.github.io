---
title: 解决 go get 下载包失败的问题
date: 2019-10-22
author: "me"
tags: ["Golang", "原创"]
categories: ["后端"]
---

## What

在使用 `go get`, `go mod` 等命令时，会自动下载相应的包或依赖包。但由于被墙，如 `golang.org/x/...` 的包会出现下载失败的情况。嗯 ... `timeout`

## How

### 手动下载

常见包如 `golang.org/x/...` 的包，通常在 github 上都有官方对应的镜像仓库，我们可以手动下载或 clone 对应 Github 仓库至指定目录下，如`golang.org/x/`，需手动创建，参考代码操作如下：

```
mkdir $GOPATH/src/golang.org/x
cd $GOPATH/src/golang.org/x
git clone git@github.com:golang/text.git
rm -rf text/.git
```

### 设置代理

如果你有代理，可设置对应的环境变量：

```
export http_proxy=http://proxyAddress:port
export https_proxy=http://proxyAddress:port
```

或直接用 all_proxy：

```
export all_proxy=http://proxyAddress:port
```

### go mod replace

Go 1.11 版本开始，新增支持 `go modules` 用于解决包依赖管理的问题。该工具提供了 `replace`，就是为了解决包的别名问题。

> 由于 `go mod` 使用不在本篇讨论范围，可参考此文 [The Go Blog: Using Go Modules](https://blog.golang.org/using-go-modules)
> 有打算之后写篇专门介绍 `go mod` 的

**注意：**

1. 小于 Go 1.11 的版本不支持，请使用上面的方式
2. 如果你的代码库在 $GOPATH 中，功能是默认不开启的，需通过环境变量开启：`export GO111MODULE=on`。

### 更改环境变量为共用代理服务

其实上述方法都有其不方便之处，从 Go 1.11 版本开始，官方除了支持`go mod`，还新增了 `GOPROXY` 环境变量。如果设置了该变量，下载源代码时将会通过这个环境变量设置的代理地址，并且提供了公用的代理服务 `https://goproxy.io`，我们只需设置该环境变量即可正常下载被墙的源码包了：

```
export GOPROXY=https://goproxy.io
```

对于 Windows 用户，可以在 PowerShell 中设置 (本人未尝试，参考使用)：

```
$env:GOPROXY = "https://goproxy.io"
```

**注意：** 注意点同上

P.S 七牛也提供了国内代理 `goproxy.cn`，大家也可使用

