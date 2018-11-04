---
layout: post
title: "iOS开发 Xcode项目兼容MRC与ARC文件"
subtitle: ""
author: "Jack"
date: 2014-04-08
tag: [iOS, Xcode, ARC, MRC]
header-img: "img/post-img/post-bg-ios.jpg"
---

## 解决方案：
- 如果你的项目使用的是MRC模式，则为ARC模式的代码文件加入`-fobjc-arc`标签。
- 如果你的项目使用的是ARC模式，则为MRC模式的代码文件加入`-fno-objc-arc`标签。

***注：添加标签方法如下：***

1. 打开target->Build Phases->Compile Sources
2. 双击对应的*.m文件
3. 在弹出的窗口中输入对应标签：`-fobjc-arc`或`-fno-objc-arc`
4. 点击Done保存