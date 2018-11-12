---
layout: post
title: "Python｜学习笔记（一）- 第一个Python程序"
subtitle: "Python学习笔记系列"
author: "Jack"
date: 2015-01-02
tag: [Python]
header-img: "img/post-img/post-bg-unix.jpg"
---

### 前言

>  本篇是Python学习笔记系列的第（一）篇，主要介绍了如何安装运行Python，以及如何编码运行第一个Python程序。

### 安装Python

Python是跨平台的，可以运行在Windows、Mac以及各种Linux/Unix系统上。在Mac上写的Python程序，放到Windows上也是能够运行的。

要开始编写Python程序，首先得安装Python。安装完成后，将会得到：

- Python解释器（负责运行Python程序）
- 一个命令行交互环境
- 一个简单的集成开发环境

##### Python3.0

目前Python有两个版本，2.x版本和3.x版本，这两个版本是不兼容的。由于3.x版本是趋势，所以一般会选择3.x版本进行安装学习。安装方法有两个：

- 从[Python官网](https://www.python.org/)下载安装
- 如果安装了Homebrew，直接通过命令 `brew install python3` 安装即可

##### 运行Python

安装成功后，打开cmd命令行，输入 `Python3` 点击回车即可进入Python交互环境，在该环境中输入任何Python代码，回车后都会立刻得到执行结果。输入 `exit()` 并回车，即可推出Python交互环境。

##### Python解释器

当我们编写完Python代码，我们得到的是一个包含Python代码的以 `.py` 为扩展名的文本文件，需要通过Ptyhon解释器来执行。

- CPython

  这是官方版本的解释器，在我们安装完Python之后就直接获取了。因为是用C语言进行开发的，所以叫CPython。在命令行下运行 `Python` 其实就是启动CPython解释器。

  这是使用最广的解释器。

- IPython、PyPy、Jython、IronPython

  除了CPython还有上述这些解释器，不再一一说明。

### 第一个Python程序

打开你的文本编辑器（推荐sublime Text），输入如下代码（*注意，`print` 前面不要有任何空格*）：

```python
print('hello Python')
```

保存文件为 `hello.py`（***文件必须要以`.py`结尾，且文件名只能是英文字母、数字和下划线的组合***），打开命令行窗口，使用 `cd` 命令切换到 `hello.py` 文件所在目录后执行：

```
python hello.py
```

##### 直接运行Python文件

那么能不能和普通程序一样，直接双击就执行Python文件呢？答案是肯定的（Mac/Linux上可以，Windows上不行）。

我们只需要在 `.py` 文件的第一行加上一个特殊的注释：

```
#!/usr/bin/env python3
```

然后通过如下命令授权 `.py` 文件执行权限：

```
$ chmod a+x hello.py
```

完成这两步就能直接运行 `.py` 程序啦！（将文件直接拖动到命令行内即可执行）

### 总结

Python跨平台的特性非常适合用来编写脚本程序，当然Python的作用远不止于此，随着学习的深入，你会对Python越发爱不释手！
