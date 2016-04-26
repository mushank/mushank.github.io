---
title: "Xcode项目中兼容MRC与ARC文件"
date: 2014-03-23 14:46:00
tag: [iOS, Xcode, MRC, ARC]
blog: true
layout: post

---

## 解决方案：
- 如果你的项目使用的是MRC模式，则为ARC模式的代码文件加入`-fobjc-arc`标签。
- 如果你的项目使用的是ARC模式，则为MRC模式的代码文件加入`-fno-objc-arc`标签。

#### 注：如何添加标签：
1. 打开target->Build Phases->Compile Sources
2. 双击对应的*.m文件
3. 在弹出的窗口中输入对应标签：`-fobjc-arc`或`-fno-objc-arc`
4. 点击Done保存