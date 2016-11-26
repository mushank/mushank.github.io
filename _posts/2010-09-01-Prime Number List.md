---
layout: post
title: "Prime Number List"
subtitle: "C语言实现"
author: "Jack"
date: 2010-09-01
tag: [PAT, C]
header-img: "img/post-img/post-bg-unix.jpg"
---

## 一、问题描述
欲构造n以内（不含n）的素数表，分析如下：

1. 令x为2；
2. 将2x，3x，4x直至ix<n的数标记为非素数；
3. 令x为下一个没有被标记为非素数的数，重复2；直到所有的数都标记完毕

伪代码描述：

1. 开辟prime[n]，初始化其所有数为1，prime[x]=1表示x为素数；
2. 令x=2；
3. 若x为素数，则对(i=2,ix<n,i++)，令prime[ix]=0，即标记ix为非素数；
4. 令x++，若x<n则重复3，否则结束

---

## 二、代码实现
C语言代码如下：

```
#include <stdio.h>  
  
int main(int argc, const char * argv[]) {  
  
    const int n = 100;  
    // 1、开辟prime[n]，初始化其所有数为1，prime[x]=1表示x为素数；  
    int prime[n];  
    for (int i = 0; i < n; i++) {  
        prime[i] = 1;  
    }  
      
    int x = 2;  // 2、令x=2  
    while (x < n) {  
        if (prime[x]) {  
            for (int i = 2; i*x < n; i++) {  
                prime[i*x] = 0; // 3、若x为素数，则对(i=2,ix<n,i++)，令prime[ix]=0，即标记ix为非素数；  
            }  
        }  
        x++;    // 4、令x++，若x<n则重复3，否则结束  
    }  
  
    for (int i = 2; i < n; i++) {  
        if (prime[i]) {  
            printf("%d\t",i);  
        }  
    }  
    printf("\n");  
      
    return 0;  
}  

```