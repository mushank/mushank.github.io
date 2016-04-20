---
title: "如何控制UIWebView不让其弹出选择复制菜单"
date: 2015-09-14 10:10:00
tag: [VIM]
blog: true
layout: post

---

代码如下：

```
[webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitUserSelect='none';"];  
[webView stringByEvaluatingJavaScriptFromString:@"document.documentElement.style.webkitTouchCallout='none';"]; 
```