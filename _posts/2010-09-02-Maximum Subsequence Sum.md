---
layout: post
title: "Maximum Subsequence Sum"
subtitle: "C语言实现"
author: "Jack"
date: 2010-09-02
tag: [PAT, C]
header-img: "img/post-img/post-bg-unix.jpg"
---

### 一、问题描述

给定N个整数的序列{A1,A2······An}，求f(i,j) = max(连续子序列和)。// 数学公式不会打，只能用语言描述了= =！

### 二、代码实现

##### 算法一： 循环遍历

这是最为常见的一种求解方式，采用循环遍历的方式求出最大连续子列和，T(n) = O(n^2)

```c
int maxSubSeqSum(int A[], int N){
    int thisSum = 0;
    int maxSum = 0;
    for (int i = 0; i < N; i++) {
        for (int j = i; j < N; j++) {
            thisSum += A[j];
            if (thisSum > maxSum) {
                maxSum = thisSum;
            }
        }
        thisSum = 0;
    }

    return maxSum;
}
```

##### 算法二：分而治之

采用分治的思想，通过递归进行求解，T(n) = O(n*Logn)

```c
int maxSubSeqSum(int A[], int leftIndex, int rightIndex){
    if (leftIndex == rightIndex) {
        return A[0];
    }
    int center = (leftIndex + rightIndex) / 2;
    int maxLeftSum = maxSubSeqSum(A, leftIndex, center);
    int maxRightSum = maxSubSeqSum(A, center + 1, rightIndex);
    
    
    int leftBorderSum = 0;
    int leftBorderMaxSum = 0;
    for (int i = center; i >= leftIndex; i--) {
        leftBorderSum += A[i];
        if (leftBorderSum > leftBorderMaxSum) {
            leftBorderMaxSum = leftBorderSum;
        }
    }
    int rightBorderSum = 0;
    int rightBorderMaxSum = 0;
    for (int i = center + 1; i <= rightIndex; i ++) {
        rightBorderSum += A[i];
        if (rightBorderSum > rightBorderMaxSum) {
            rightBorderMaxSum = rightBorderSum;
        }
    }
    int maxBorderSum = leftBorderMaxSum + rightBorderMaxSum;
    
    int maxSum = maxLeftSum > maxRightSum ? maxLeftSum : maxRightSum;
    maxSum = maxSum > maxBorderSum ? maxSum : maxBorderSum;
    
    return maxSum;
}
```

##### 算法三：在线处理

这是效率最高的问题求解方式，T(n) = O(n)

***在线处理是指每输入一个数据就进行即时处理，在任意位置终止输入，算法都能正确给出当前的解***

```c
int maxSubSeqSum(int A[], int N){
    int thisSum = 0;
    int maxSum = 0;
    for (int i = 0; i < N; i++) {
        thisSum += A[i];
        if (thisSum > maxSum) {
            maxSum = thisSum;
        }else if(thisSum < 0){
            thisSum = 0;
        }
    }
    
    return maxSum;
}
```

综上，三种方法时间复杂度从高到低，在线处理的方式为最优算法。