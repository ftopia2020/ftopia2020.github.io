---
title: Python 版本管理器 —— pyenv 简介及使用
date: 2019-09-02
author: "me"
tags: ["Python", "pyenv", "原创"]
categories: ["后端"]
---

## Why

**!注意：若你使用的是Windows，请忽略本文，pyenv不支持Windows，原因是作者大神不想浪费时间** 
> I'm no longer using Windows. I don't like to waste my time with such thing.
若是Windows系统，可使用 virtualenv 及 virtualenvwrapper

Python执行环境问题，可能常常遇到这样的情况：

- 系统自带的 Python 是 2.x，而自己需要 Python 3.x
- 需要测试不同Python版本的代码

Python2.X 和 3.X 版本，甚至众多Python版本共存于一个系统，如何简单优雅地切换Python环境？

目前一个比较成熟的方式，就是 **pyenv**。

解决了多版本问题，势必又有另一个棘手的环境问题出现，即三方库版本问题，如何确保Python独立开发环境？相信大多人会选择大名鼎鼎的virtualenv。而 pyenv 的插件 **pyenv-virtualenv** 可以同步解决这个问题。



## What

[**pyenv**](https://github.com/pyenv/pyenv) 是一个简易的Python版本管理器 （Simple Python Version Management）

> pyenv lets you easily switch between multiple versions of Python. It's simple, unobtrusive, and follows the UNIX tradition of single-purpose tools that do one thing well.

**pyenv does...**

- 用户级别的全局 Python 版本切换
- 单项目提供多个 Python 版本切换
- 使用环境变量来重写 Python 版本
- 同一时间对不同版本执行命令搜索

> - Let you **change the global Python version** on a per-user basis.
> - Provide support for **per-project Python versions**.
> - Allow you to **override the Python version** with an environment variable.
> - Search commands from **multiple versions of Python at a time**. This may be helpful to test across Python versions with [tox](https://pypi.python.org/pypi/tox).

[pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) 是pyenv用来管理虚拟环境的插件，pyenv + pyenv-virtualenv 配合使用可以让我们随意切换多版本多环境下的Python项目。

> pyenv-virtualenv is a [pyenv](https://github.com/pyenv/pyenv) plugin that provides features to manage virtualenvs and conda environments for Python on UNIX-like systems.
>
> (NOTICE: If you are an existing user of [virtualenvwrapper](http://pypi.python.org/pypi/virtualenvwrapper) and you love it, [pyenv-virtualenvwrapper](https://github.com/pyenv/pyenv-virtualenvwrapper) may help you (additionally) to manage your virtualenvs.)



## How

### Install

具体的安装就不介绍了，详见 [pyenv install](https://github.com/pyenv/pyenv#installation)，[pyenv-virtualenv install](https://github.com/pyenv/pyenv-virtualenv#installation)



### How it works

pyenv的工作原理还是比较简单的：即修改系统环境变量`PATH`，如有兴趣可查阅 [How it works](https://github.com/pyenv/pyenv#how-it-works)

而pyenv-virtualenv则是作为插件集成了virtualenv的功能



### Common Commands

#### pyenv

列出pyenv常用命令，详情请查阅 [官方CMDs](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md)

| CMD                                      | Description                       |
| ---------------------------------------- | --------------------------------- |
| pyenv commands                           | 列出所有pyenv支持的命令                    |
| pyenv CMD --help / pyenv CMD -h          | CMD命令帮助信息                         |
| pyenv version                            | 查看当前使用的Python版本，包含设置的信息           |
| pyenv versions                           | 列出当前pyenv所有可用的Python版本，*标时当前使用的版本 |
| pyenv install -- list / pyenv install - l | 列出可安装的所有Python版本信息                |
| pyenv install PYTHON_VERSION             | 安装指定的Python版本                     |
| pyenv uninstall PYTHON_VERSION           | 卸载指定的Python版本                     |
| pyenv rehash                             | 更新hash；在安装Python版本或其他可执行模块后需执行更新  |
| pyenv global PYTHON_VERSION              | 设置全局的 python 版本                   |
| pyenv local PYTHON_VERSION               | 设置本地的 python 版本                   |

日常使用示例如下：

```shell
pyenv install 3.6.0		# 安装python3.6.0版本
pyenv global 3.6.0		# !慎用，设置全局Python版本为3.6.0 
pyenv version			# 查看当前使用的Python版本
pyenv local 3.6.0		# 设置本地(即当前目录下)Python版本为3.6.0 
```



#### pyenv-virtualenv

> 熟悉virtualenv的用起来毫无难度

- 从当前版本创建virtualenv

```shell
pyenv virtualenv venv36		# 当前Python版本下创建虚拟环境 venv36
```

- 指定版本创建virtualenv

```shell
  pyenv virtualenv 2.7.12 projectX_2.7.12	  # 2.7.12版本下创建虚拟环境projectX_2.7.12
```

- 设置虚拟环境

```shell
pyenv local projectX_2.7.12		# local设置为虚拟环境projectX_2.7.12
```

设置成功后，shell命令行路径前会出现  `(projectX_2.7.12)` 标识，表示当前目录在该虚拟环境下

可试着切换目录，会发现离开了此环境；若再次回到该目录，会发现仍是在此环境下



若我们需要临时使用某个环境或是做多环境下的项目测试，可用activate及deactivate来临时使用虚拟环境，此时切换路径不会改变环境，此环境选择的优先级高于目录local设置环境

- 激活虚拟环境

```shell
pyenv activate projectX_2.7.12		# 激活当前运行环境为projectX_2.7.12
```

- 离开激活的虚拟环境

```shell
pyenv deactivate	# 离开激活的虚拟环境
```



### Q&A

**Q：** `pyenv install PYTHON_VERSION` 安装时很慢，甚至于超时，如何解决

**A：** 自行下载对应的安装包并拷贝至 ~/.pyenv/cache 目录下（通常第一次需自己新建）；于缓存中安装该版本，使用命令 `pyenv install PYTHON_VERSION -v`




**Q：** Mac使用pyenv切换版本后，发现仍只是使用了系统Python版本

**A：** 原因为找不到通过 pyenv 安装的 Python；这是homebrew安装pyenv留的一个坑。。。需要在安装pyenv后更改 ~/.bash_profile 文件（若无需新建），增加语句如下：

```
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
```

安装的时候相关提示：

> Then follow the rest of the post-installation steps under [Basic GitHub Checkout](https://github.com/pyenv/pyenv#basic-github-checkout) above, starting with #3 ("Add `pyenv init` to your shell to enable shims and autocompletion").



**Q：** global 版本、local版本的区分，优先级？

**A：** `pyenv version`的结果 () 内可区分出为哪种设置， ~/.pyenv/version 为全局的配置文件；优先local，若无后取global；local的配置文件为 {LOCAL_PATH}/.python-version，若手动删除该文件，local版本就失效了 



**Q：** MacOS pyenv install 报错，Missing the OpenSSL lib?

**A：** 由于Mac系统更新，而pyenv版本不是最新的导致报错，`brew update pyenv` 后解决该问题。



