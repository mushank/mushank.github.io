---
title: "如何自定义UINavigationBar的背景图片"
date: 2014-03-23 23:12:00
tag: [iOS, NavigationBar]
blog: true
layout: post

---

## 获取当前设备运行的iOS版本号

```
// 该函数返回当前iOS版本号
float curVersion(){
    return [[[UIDevice currentDevice]systemVersion]floatValue];
}
```

## 5.0版本开始，navigationBar开始支持自定义背景图片：

```
if(version >= 5.0){
    [self.navigationBar setBackgroundImage:[UIImage imageNamed:@"navigationbar_background.png"] forBarMetrics: (UIBarMetrics)];
}
```

## 当前版本不支持自定义时（5.0版本以下），通过添加类别来进行自定义UINavigationBar背景图片

**.h文件：**

```
#import <Foundation/Foundation.h>

@interface UINavigationBar(setBackgroud)
@end
```
**.m文件**

```
@implementation UINavigationBar(setBackgroud)

- (void)drawRect:(CGRect)rect {
    UIImage *image = [UIImage imageNamed:@"navigationbar_background.png"];
    [image drawInRect:rect];
}
@end
```