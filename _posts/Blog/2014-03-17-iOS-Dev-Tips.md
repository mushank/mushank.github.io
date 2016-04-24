---
title: "iOS Dev Tips"
date: 2014-03-17 19:30:00
tag: [iOS]
blog: true
layout: post

---

### 1. 如何将路径`/usr/local/bin`添加到系统环境变量

1. cd to `../etc/`
2. then use `ls` to make sure your `paths` file is in
3. vim `paths`, add `/usr/local/bin` at the end of the file


### 2. 如何判断判断某个日期为星期几

```
NSCalendar *calendar = [NSCalendar currentCalendar];  
NSUInteger unitFlags = NSYearCalendarUnit | NSMonthCalendarUnit | NSWeekdayCalendarUnit | NSDayCalendarUnit | NSHourCalendarUnit | NSMinuteCalendarUnit;  
NSDateComponents *dateComponent = [calendar components:unitFlags fromDate:date];  
switch ([dateComponent weekday]) {  
    case 1:{  
        NSLog(@"周日");  
        break;  
    }  
    case 2:{  
        NSLog(@"周一");  
        break;  
    }  
    case 3:{  
        NSLog(@"周二");  
        break;  
    }  
    case 4:{  
        NSLog(@"周三");  
        break;  
    }  
    case 5:{  
        NSLog(@"周四");  
        break;  
    }  
    case 6:{  
        NSLog(@"周五");  
        break;  
    }  
	case 7:{  
        NSLog(@"周六");  
        break;  
    }  

    default:  
        break;  
}  

```

### 3. 如何隐藏状态栏上的网络指示器

`[UIApplication sharedApplication].networkActivityIndicatorVisible = NO;`

***注：要注意将该状态和程序当前的请求数相对应，如果你设置属性之后状态栏还是显示指示器，很可能是因为刚设置好的属性被另一个网络请求给覆盖了。***

### 4. 如何控制UIWebView不让其弹出选择复制菜单

```
[webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitUserSelect='none';"];  
[webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitTouchCallout='none';"]; 
```

### 5. 如何修改NavigationBar中title的字体大小和颜色

```
NSDictionary *dic = [NSDictionary dictionaryWithObjectsAndKeys:[UIColor whiteColor],NSForegroundColorAttributeName,[UIFont boldSystemFontOfSize:21],NSFontAttributeName,nil];  
self.navigationBar.titleTextAttributes = dic;  
```

### 6. 如何修改UITextField中placeholder的字体颜色

```
[_textField setValue:[UIColor redColor] forKeyPath:@"_placeholderLabel.textColor"];  
```

### 7. 如何调整TextField不被软键盘遮挡

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

### 8. 如何实现UITableViewCell自适应高度

```
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath  
{  
    CGFloat yHeight = 0;  
      
    NSString *remarkContentString = @"备注是神马啊";  
    NSDictionary *attribute = @{NSFontAttributeName:[UIFont systemFontOfSize:13]};  
    CGSize size = [remarkContentString boundingRectWithSize:CGSizeMake(170.0 , MAXFLOAT) options: NSStringDrawingTruncatesLastVisibleLine | NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingUsesFontLeading attributes:attribute context:nil].size;  
      
    yHeight += 40 + size.height + 12;  
      
    return yHeight;  
      
} 
```

### 9. 如何判断一个字符串里只有纯数字

除了使用正则表达式，苹果还给我们提供了一个相当好用的函数，帮助我们实现相关功能：

```
#pragma mark - PureInt && PureFloat  
- (BOOL)isPureInt:(NSString*)string{  
    NSScanner* scan = [NSScanner scannerWithString:string];  
    int val;  
    return[scan scanInt:&val] && [scan isAtEnd];  
}  
  
- (BOOL)isPureFloat:(NSString*)string{  
    NSScanner* scan = [NSScanner scannerWithString:string];  
    float val;  
    return [scan scanFloat:&val] && [scan isAtEnd];  
}  
```

### 10. 如何控制只允许用户输入数字

```
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string  
{  
    if (textField == totalAmountTextField)  
    {  
        return [self validateNumber:string];  
    }  
    return YES;  
}  
  
- (BOOL)validateNumber:(NSString*)number {  
    BOOL res = YES;  
    NSCharacterSet* tmpSet = [NSCharacterSet characterSetWithCharactersInString:@"0123456789"];  
    int i = 0;  
    while (i < number.length) {  
        NSString * string = [number substringWithRange:NSMakeRange(i, 1)];  
        NSRange range = [string rangeOfCharacterFromSet:tmpSet];  
        if (range.length == 0) {  
            res = NO;  
            break;  
        }  
        i++;  
    }  
    return res;  
}  
```



