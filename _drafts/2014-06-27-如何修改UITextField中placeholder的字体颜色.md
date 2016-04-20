---
title: "如何修改UITextField中placeholder的字体颜色"
date: 2014-06-27 17:44:00
tag: [iOS, UITextField]
blog: true
layout: post

---

1、如何修改UITextField中placeholder的字体颜色呢，其实很简单，只要如下一行代码：

```
[_textField setValue:[UIColor colorWithRed:143/255.0 green:93/255.0 blue:40/255.0 alpha:1.0] forKeyPath:@"_placeholderLabel.textColor"];  
```
2、如何修改NavigationBar中title的字体大小和颜色

```
// 修改title字体大小和颜色  
UIColor * titleColor = [UIColor whiteColor];  
NSDictionary *dic = [NSDictionary dictionaryWithObjectsAndKeys:titleColor,NSForegroundColorAttributeName,[UIFont boldSystemFontOfSize:21],NSFontAttributeName,nil];  
self.navigationBar.titleTextAttributes = dic;  
```