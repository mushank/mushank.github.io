---
title: "NSClassFromString"
date: 2015-10-23 13:55:00
tag: [VIM]
blog: true
layout: post

---

NSClassFromString是一个很有用的东西，尤其在进行iPhone toolchain的开发上。

正常来说，

id myObj = [[NSClassFromString(@"MySpecialClass") alloc] init];

和

id myObj = [[MySpecialClass alloc] init];

是一样的。但是，如果你的程序中并不存在MySpecialClass这个类，下面的写法会出错，而上面的写法只是返回一个空对象而已。

因此，在某些情况下，可以使用NSClassFromString来进行你不确定的类的初始化。

比如在iPhone中，NSTask可能就会出现这种情况，所以在你需要使用NSTask时，最好使用：

[[NSClassFromString(@"NSTask") .....]]

而不要直接使用[NSTask ...]这种写法。

NSClassFromString的好处是：

1 弱化连接，因此并不会把没有的Framework也link到程序中。

2 不需要使用import，因为类是动态加载的，只要存在就可以加载。因此如果你的toolchain中没有某个类的头文件定义，而你确信这个类是可以用的，那么也可以用这种方法。

文章转自：http://www.cocoachina.com/mac/20090611/231.html