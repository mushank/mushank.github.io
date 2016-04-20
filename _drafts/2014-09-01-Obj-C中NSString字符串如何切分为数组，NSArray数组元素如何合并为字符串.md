---
title: "Obj-C中NSString字符串如何切分为数组，NSArray数组元素如何合并为字符串"
date: 2014-09-01 14:28:00
tag: [iOS]
blog: true
layout: post

---

如果你曾经使用过Python这种脚本语言，那么你可能已经习惯于将字符串切分为数组和将数组元素合并成字符串这种操作了.在OC中NSArray也可以执行这种操作。

使用-componentsSeparatedByString:可以切分NSArray，像这样：

```
NSString *string = @"oop:ack:bork:greeble:ponies";  
NSArray *array = [string componentsSeparatedByString:@":"];  
```

还可以使用componentsJoinedByString:来合并NSArray中的元素并创建字符串：

```
string = [array componentsJoinedByString:@":-）"];  
```
上面的代码行将会创建一个内容为“oop:-)ack:-)bork:-)greeble:-)ponies”的NSString字符串
