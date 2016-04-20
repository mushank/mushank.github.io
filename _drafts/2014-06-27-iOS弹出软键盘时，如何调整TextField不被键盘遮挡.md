---
title: "iOS弹出软键盘时，如何调整TextField不被键盘遮挡"
date: 2014-06-27 16:18:00
tag: [iOS, Keyboard, Animation]
blog: true
layout: post

---

在iOS开发中，由于屏幕尺寸的限制，经常会遇到弹出的软键盘遮挡住TextField从而导致用户看不见输入内容的情况，这里提供一种解决方法：

```
// textField上移动画  
- (void)textFieldAnimate:(UITextField *)textField isUp:(BOOL)isUp  
{  
    int movementDistance = 140; // 根据需要调整平移距离  
    float movementDuration = 0.3f;  
    int movement = (isUp ? -movementDistance : movementDistance);  
      
    [UIView beginAnimations: @"textFieldAnimation" context: nil nil];  
    [UIView setAnimationBeginsFromCurrentState:YES];  
    [UIView setAnimationDuration:movementDuration];  
    self.view.frame = CGRectOffset(self.view.frame, 0, movement);  
    [UIView commitAnimations];  
}
  
#pragma mark - TextField Delegate  
- (void)textFieldDidBeginEditing:(UITextField *)textField  
{  
    [self textFieldAnimate:textField isUp:YES];  
}
  
- (void)textFieldDidEndEditing:(UITextField *)textField  
{  
    [self textFieldAnimate:textField isUp:NO];  
}
```
