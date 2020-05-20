---
title: Mac 安装和使用 brew (Homebrew —— Mac 包管理工具)
date: 2020-01-14
author: "me"
tags: ["mac", "homebrew", "原创"]
categories: ["系统&工具"]
---


## What

Homebrew 简称 brew，是 Mac OSX 上的软件包管理工具，支持在 Mac 中方便的安装软件或者卸载软件，可以说 Homebrew 就是 mac 下的 apt-get、yum神器。

Homebrew 官网：https://brew.sh/ （安装后可 `brew home` 用浏览器打开 brew 的官方网站）

## How

### Install

Homebrew 的安装非常简单，打开终端，复制、粘贴以下命令，回车。（From 官网）

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Using

Homebrew 的使用类似 yum，常用命令如下（更多信息请访问官网）：

搜索软件：`brew search {软件名}`，如 brew search wget；brew search /wge*/ 支持类正则模糊搜索

查看软件：`brew info {软件名}`，如brew info wget

安装软件：`brew install {软件名}`，如brew install wget

更新软件：`brew upgrade {软件名}`，如brew upgrade wget

卸载软件：`brew remove 软件名`，如brew remove wget

更新Homebrew：`brew update`

清理软件：`brew cleanup` ，清理不需要的软件版本及其安装包缓存

更多详见官网文档：https://docs.brew.sh/

