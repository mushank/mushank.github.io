---
title: "如何自定义UINavigationBar的背景图片"
date: 2014-03-23 23:12:00
tag: [iOS, UINavigationBar]
blog: true
layout: post

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