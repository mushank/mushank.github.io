---
title: "iOS网络访问：同步请求、异步请求、GET请求、POST请求"
date: 2014-07-08 14:35:00
tag: [iOS]
blog: true
layout: post

---

1、同步请求可以从因特网请求数据，一旦发送同步请求，程序将停止用户交互，直至服务器返回数据完成，才可以进行下一步操作，

2、异步请求不会阻塞主线程，而会建立一个新的线程来操作，用户发出异步请求后，依然可以对UI进行操作，程序可以继续运行

3、GET请求，将参数直接写在访问路径上。操作简单，不过容易被外界看到，安全性不高，地址最多255字节；

4、POST请求，将参数放到body里面。POST请求操作相对复杂，需要将参数和地址分开，不过安全性高，参数放在body里面，不易被捕获。

```
/*---------同步GET请求----------*/  
  
//第一步，创建URL  
NSURL *url = [NSURL URLWithString:@"http://api.hudong.com/iphonexml.do?type=focus-c"];  
  
//第二步，通过URL创建网络请求  
NSURLRequest *request = [[NSURLRequest alloc]initWithURL:url cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10];  
//NSURLRequest初始化方法第一个参数：请求访问路径，第二个参数：缓存协议，第三个参数：网络请求超时时间（秒）  
  
//其中缓存协议是个枚举类型包含：  
//NSURLRequestUseProtocolCachePolicy（基础策略）  
//NSURLRequestReloadIgnoringLocalCacheData（忽略本地缓存）  
//NSURLRequestReturnCacheDataElseLoad（首先使用缓存，如果没有本地缓存，才从原地址下载）  
//NSURLRequestReturnCacheDataDontLoad（使用本地缓存，从不下载，如果本地没有缓存，则请求失败，此策略多用于离线操作）  
//NSURLRequestReloadIgnoringLocalAndRemoteCacheData（无视任何缓存策略，无论是本地的还是远程的，总是从原地址重新下载）  
//NSURLRequestReloadRevalidatingCacheData（如果本地缓存是有效的则不下载，其他任何情况都从原地址重新下载）  
  
//第三步，连接服务器  
NSData *received = [NSURLConnection sendSynchronousRequest:request returningResponse:nil error:nil];  
NSString *str = [[NSString alloc]initWithData:received encoding:NSUTF8StringEncoding];  
NSLog(@"%@",str);  
  
/*---------同步POST请求----------*/  
  
//第一步，创建URL  
NSURL *url = [NSURL URLWithString:@"http://api.hudong.com/iphonexml.do"];  
  
//第二步，创建请求  
NSMutableURLRequest *request = [[NSMutableURLRequest alloc]initWithURL:url cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10];  
[request setHTTPMethod:@"POST"];//设置请求方式为POST，默认为GET  
NSString *str = @"type=focus-c";//设置参数  
NSData *data = [str dataUsingEncoding:NSUTF8StringEncoding];  
[request setHTTPBody:data];  
  
//第三步，连接服务器  
NSData *received = [NSURLConnection sendSynchronousRequest:request returningResponse:nil error:nil];  
NSString *str1 = [[NSString alloc]initWithData:received encoding:NSUTF8StringEncoding];  
NSLog(@"%@",str1);  
  
/*---------异步GET请求----------*/  
  
//第一步，创建url  
NSURL *url = [NSURL URLWithString:@"http://api.hudong.com/iphonexml.do?type=focus-c"];  
  
//第二步，创建请求  
NSURLRequest *request = [[NSURLRequest alloc]initWithURL:url cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10];  
  
//第三步，连接服务器  
NSURLConnection *connection = [[NSURLConnection alloc]initWithRequest:request delegate:self];  
  
/*---------异步POST请求----------*/  
  
//第一步，创建url  
NSURL *url = [NSURL URLWithString:@"http://api.hudong.com/iphonexml.do"];  
  
//第二步，创建请求  
NSMutableURLRequest *request = [[NSMutableURLRequest alloc]initWithURL:url cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10];  
[request setHTTPMethod:@"POST"];  
NSString *str = @"type=focus-c";  
NSData *data = [str dataUsingEncoding:NSUTF8StringEncoding];  
[request setHTTPBody:data];  
  
//第三步，连接服务器  
NSURLConnection *connection = [[NSURLConnection alloc]initWithRequest:request delegate:self];  
  
//异步请求的代理方法  
  
//接收到服务器回应的时候调用此方法  
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response  
{  
    NSHTTPURLResponse *res = (NSHTTPURLResponse *)response;  
    NSLog(@"%@",[res allHeaderFields]);  
    self.receiveData = [NSMutableData data];  
}  
  
//接收到服务器传输数据的时候调用，此方法根据数据大小执行若干次  
-(void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data  
{  
    [self.receiveData appendData:data];  
}  
  
//数据传完之后调用此方法  
-(void)connectionDidFinishLoading:(NSURLConnection *)connection  
{  
    NSString *receiveStr = [[NSString alloc]initWithData:self.receiveData encoding:NSUTF8StringEncoding];  
    NSLog(@"%@",receiveStr);  
}  
  
//网络请求过程中，出现任何错误（断网，连接超时等）会进入此方法  
-(void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error  
{  
    NSLog(@"%@",[error localizedDescription]);  
}  
```