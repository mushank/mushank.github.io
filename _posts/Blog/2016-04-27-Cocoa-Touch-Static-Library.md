---
title: "Cocoa Touch Static Library"
date: 2016-04-27 16:26:42
tag: [iOS, Static Library]
blog: true
layout: post

---

## 简述

静态库通常用于隐藏功能实现代码，使用者只能通过头文件调用相关接口，而无法知道其中的实现细节。同样的还有动态库（动态链接库），两者之间的主要区别在于：静态库可以编译到我们的应用程序中，应用程序可以在没有静态库的环境中运行；而动态库不能被编译到我们的应用程序中，应用程序必须在有动态库的环境下才能正常运行。

## 创建工程

> Xcode -> File -> New -> Project, 在弹出的工程创建选项中，选择`Cocoa Touch Static Library`, 填入相关信息完成静态库工程的创建。

## 编译静态库

1. 选择`iOS Simulator` -> Command+B, 编译成功后，工程Projcets结构下显示的`*.a`文件即是编译成功的静态库文件，右键`Show in Finder` 
2. 选择`Build Only Device` -> Command+B, 编译成功后，工程Projcets结构下显示的`*.a`文件即是编译成功的静态库文件，右键`Show in Finder`

	注：步骤1. 2.中分别编译生成了两份静态库文件，这是因为在`iOS Simulator`环境下编译的静态库只能供模拟器使用，在`Build Only Device`环境下编译的静态库只能供真机环境使用。可以使用命令`$ lipo -info <lib.a>`查看对应静态库文件的属性，两个库文件对应的信息如下：
	
	- iOS Simulator
	
		>	rchitectures in the fat file: lib.a are: armv7 arm64
	
	- Build Only Device
	
		>	input file libHTMF.a is not a fat file  
			Non-fat file: libHTMF.a is architecture: x86_64
		

## 合并静态库

1. 为了方便使用，通常会将编译生成的两个静态库文件（模拟器/真机）合并成一个文件进行使用。合并后的文件体积会有所变大，一般情况下为先前两个文件的和，所以实际项目发版时，建议只导入真机使用静态库文件。合并命令如下：  
`$ lipo -create <iphoneos-lib.a> <iphonesimulator-lib.a> -output <lib.a>`

## 使用静态库
将`.a`、`.h`、`资源文件`拖入实际项目，在想使用静态库的类中导入头文件，即可使用。








