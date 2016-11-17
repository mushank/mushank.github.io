---
layout: post
title: "自定义UINavigationBar的背景图片"
subtitle: ""
author: "Jack"
date: 2014-03-22
tag: [iOS]
header-img: "img/post-img/post-bg-ios.jpg"
---



## 1. iOS5.0版本开始，navigationBar开始支持自定义背景图片：

```
[self.navigationBar setBackgroundImage:[UIImage imageNamed:@"backgroundImage.png"] forBarMetrics: (UIBarMetrics)];
```

## 2. iOS5.0版本以下，可以通过给navigationBar添加类别的方式进行自定义背景图片

```
UINavigationBar+setBackgroundImage.h

#import <Foundation/Foundation.h>

@interface UINavigationBar(setBackgroundImage)
@end


UINavigationBar+setBackgroundImage.m

@implementation UINavigationBar(setBackgroundImage)

- (void)drawRect:(CGRect)rect {
    UIImage *image = [UIImage imageNamed:@"backgroundImage.png"];
    [image drawInRect:rect];
}
@end
```