---
title: "创建iOS静态库Cocoa Touch Static Library"
date: 2016-04-27 16:26:42
tag: [iOS, Static Library]
blog: true
layout: post

---

1. 生成的静态库分为模拟器使用x386，真机设备使用arm7/arm64使用，可使用终端命令lipo -create进行合并，合并后的静态库体积略微增加，但是可以在真机和模拟器双重环境下运行
2. 静态库中的资源文件必须放在根目录下，不在根目录下的资源文件在使用静态库时会报错『"_OBJC_CLASS_$_ZKUDIDManager", referenced from:』
3. 创建静态库时，请指定需要支持的iOS最低版本，默认是支持最新版本iOS系统，而通常我们的应用需要支持的版本不仅仅是最新版本iOS系统