---
layout: post
title: "C++ / STL / Vector容器实现词频统计与排序"
subtitle: ""
author: "Jack"
date: 2011-12-10
tag: [C++, STL, Vector]
header-img: "img/post-img/post-bg-unix.jpg"
---

采用STL中的Vector容器实现词频统计与排序，具体代码如下：

```
#include <iostream>  
#include <vector>  
#include <string>  
#include <algorithm>  
  
using namespace std;  
  
int main()  
{  
    string strInput = "我 爱 吃 苹果 ， 我 更 爱 吃 香蕉 。 ";  
  
    vector<string>vecWords;  
    unsigned int pos1 = 0;  
    unsigned int pos2 = 0;  
    pos2 = strInput.find(" ", pos1);  
    string strWord;  
    while(pos2 != string::npos)  
    {  
        strWord = strInput.substr(pos1, pos2 - pos1);  
        vecWords.push_back(strWord);  
  
        pos1 = pos2 + 1;  
        pos2 = strInput.find(" ", pos1);  
    }  
  
    sort(vecWords.begin(), vecWords.end());  
  
  
    unsigned int n = 0;  
    int nCount = 1;  
    for(n = 0; n < vecWords.size()-1; ++n)  
    {  
        if(vecWords[n] == vecWords[n+1])  
        {  
            nCount++;  
        }  
        else  
        {  
            cout << vecWords[n] << "        " << nCount << endl;  
            nCount = 1;  
        }  
    }  
  
    if(vecWords[vecWords.size()-2] != vecWords[vecWords.size()-1])  
        cout << vecWords[vecWords.size()-1] << "      " << 1 << endl;  
}  
```