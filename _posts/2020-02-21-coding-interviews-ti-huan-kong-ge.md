---
layout: post
title: 剑指Offer 05 题解
description: "替换空格"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

### 题解
1. 法一：直接使用`StringBuffer`拼接字符串。
```java
class Solution {
    public String replaceSpace(String s) {
        StringBuffer res = new StringBuffer();
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == ' ')
                res.append("%20");
            else
                res.append(c);
        }
        return res.toString();
    }
}
```
* 此题的重点在于不适用额外空间完成，则在输入为`StringBuffer`时，可先将输入字符串进行扩充，再通过双指针从后往前遍历。
```java
public String replaceSpace(StringBuffer str) {
    int P1 = str.length() - 1;
    for (int i = 0; i <= P1; i++)
        if (str.charAt(i) == ' ')
            str.append("  ");

    int P2 = str.length() - 1;
    while (P1 >= 0 && P2 > P1) {
        char c = str.charAt(P1--);
        if (c == ' ') {
            str.setCharAt(P2--, '0');
            str.setCharAt(P2--, '2');
            str.setCharAt(P2--, '%');
        } else {
            str.setCharAt(P2--, c);
        }
    }
    return str.toString();
}
```