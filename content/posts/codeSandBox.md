---
title: 前端在线IDE —— CodeSandBox，不仅是在线代码编辑器（非广告贴，包含简介及使用）
date: 2019-09-17
author: "me"
tags: ["CodeSandBox", "原创"]
categories: ["前端"]
---

## Why

我们在前端学习或者项目开发中，时常会敲些 demo 来查看效果或者做些新鲜的尝试，这通常需要引入各种依赖以及加入不同的三方库。
如果都在自己的项目或者干脆启新项目，难免造成干扰或不便捷。如能直接在线敲Demo并即时查看效果，那该多好~

## What

在线编辑器其实大家并不陌生，教程类网站通常会结合在线编辑器，简单的如 w3schools。

这里，个人推荐一款强大的前端在线编辑平台 —— CodeSandBox，官网地址：[https://codesandbox.io](https://codesandbox.io/)

正如标题所言，CodeSandBox 已经不仅仅是编辑器，而是一个开箱即用的Web在线IDE，目前提供了几乎全部的主流框架，参考下图（Before 2019.9）

![CodeSandBox_Templates](http://image.ftopia.cn/blog/CodeSandBox_Templates.png)

通过选择不同的模板，我们可以直接使用不同框架（React、Vue、Angular等）下的环境并进行预览和测试。

在各个项目中，通过 Add Dependency，可以引入各种不同版本的依赖库，并且可以方便的分享及加入团队合作等（部分功能需要付费，个人使用完全够用了）。

> 更多类似平台可参考：[前端必知 - 各种在线 web 编辑器平台简要对比介绍 From 知乎](https://zhuanlan.zhihu.com/p/37490345)

## How

那我们如何使用 CodeSandBox 呢？如果不是前端初学者，使用上应该不难理解，当然英文界面可能会对英文不好的同学造成一些阻碍。

我们需要一个账号登录，可使用 Github 账号联合登录。之后在个人 Dashboard 中，可创建新的 SandBox，或者对 SandBoxes 进行管理或再次编辑。

尝试创建一个新的 React SandBox，稍加等待后，我们可看到一个 Hello 的示例。
![CodeSandBox_new](http://image.ftopia.cn/blog/CodeSandBox_new.png)

默认界面分为三部分，风格类似很多IDE，如VSCode等：

- 左侧一栏包含SandBox基础信息，如私有/公开，名称等，这些都可以进行设置。下方 Files & Dependencies 可方便的管理文件及依赖。
- 编辑界面为默认选中的文件，可通过点选打开对应文件，双击保留文件并支持多Tab切换，可支持分屏。
- 默认右侧栏显示浏览器预览界面，可关闭/展开。Tests可执行写入的单元测试及测试集。

可尝试修改某些参数，查看预览显示来初步体验，预览是跟随编辑实时更新的，而非保存后更新。编辑器中支持代码美化、提示联想等（使用体验和IDE真没多大差），甚至支持 Eslint。

最后，再举个例子，https://codesandbox.io/s/rro676q08p 。打开页面后，我们可以看到一个基于 antd的示例，实际上目前所有 antd 组件的示例都可于 COdeSandBox 中打开，如果使用此组件集，可以很方便的调试。