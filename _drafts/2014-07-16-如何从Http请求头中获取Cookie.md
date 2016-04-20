---
title: "如何从Http请求头中获取Cookie"
date: 2014-07-16 14:14:00
tag: [iOS]
blog: true
layout: post

---

以登录为例，网络访问使用AFNetworking封装库，代码如下：

```
NSDictionary *dic = @{@"userName": userNameString,  
                      @"password": passWordString};  
AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];  
manager.requestSerializer = [AFJSONRequestSerializer serializer];  
manager.responseSerializer.acceptableContentTypes = [NSSet setWithObjects:@"text/plain", @"application/json", nil nil];  
[manager.requestSerializer setValue:@"XMLHttpRequest" forHTTPHeaderField:@"X-Requested-With"];  
[manager.requestSerializer setValue:@"FAKEFORM" forHTTPHeaderField:@"x-form-id"];  
[manager.requestSerializer setValue:@"json" forHTTPHeaderField:@"dataType"];  
  
[manager POST:@"http://172.17.3.109/app/user/login" parameters:dic success:^(AFHTTPRequestOperation *operation, id responseObject) {  
    NSLog(@"AFsuccess: %@", operation.responseString);  
    NSDictionary *receivedDic = [NSJSONSerialization JSONObjectWithData:operation.responseData options:NSJSONReadingMutableLeaves error:nil];  
    NSString *successInfo = [receivedDic objectForKey:@"code"];  
    // 查看表头信息  
    if ([operation.response respondsToSelector:@selector(allHeaderFields)]) {  
        // 取得所有请求的头  
        NSDictionary *dictionary = [operation.response allHeaderFields];  
        NSLog(@"%@",dictionary);  
        // 取得http状态码  
        //NSLog(@"%d",[operation.response statusCode]);  
        // 处理字符串，取得所需ST值  
        NSString *set_Cookie_String = [dictionary objectForKey:@"Set-Cookie"];  
        NSLog(@"Set-Cookie:%@",set_Cookie_String);  
        for (int i = set_Cookie_String.length - 3; i>=0; i--) {  
            if ([[set_Cookie_String substringWithRange:NSMakeRange(i, 3)] isEqualToString:@"ST="]) {  
                NSString *subString = [set_Cookie_String substringWithRange:NSMakeRange(i + 3, set_Cookie_String.length - i - 3)];  
                for (int j = 0; j < subString.length; j++) {  
                    if ([[subString substringWithRange:NSMakeRange(j, 1)] isEqualToString:@";"]){  
                        NSString *st_String = [subString substringToIndex:j];  
                        NSLog(@"st_String:%@",st_String);  
                        //本地存储cookie  
                        [[NSUserDefaults standardUserDefaults] setObject:st_String forKey:@"set_cookie"];  
                        [[NSUserDefaults standardUserDefaults] synchronize];  
                        break;  
                    }  
                }  
                break;  
            }  
        }  
    }  
      
    //NSLog(@"%@",successInfo);  
    if ([successInfo isEqualToString:@"ACK"]) {  
        [[NSNotificationCenter defaultCenter] postNotificationName:NOTI_LOGIN_SUCCEEDED object:nil];  
    }else if([successInfo isEqualToString:@"VALIDATION_ERROR"]){  
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:nil message:@"用户名或密码错误，请重新输入!" delegate:nil cancelButtonTitle:@"确定" otherButtonTitles:nil, nil nil];  
        [alert show];  
    }  
} failure:^(AFHTTPRequestOperation *operation, NSError *error) {  
    NSLog(@"AFres: %@", operation.responseString);  
    NSLog(@"AFerror : %@", error);  
}];  
```