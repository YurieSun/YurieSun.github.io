---
layout: post
title: LeetCode 0367 题解
description: "有效的完全平方数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

### 题解
1. 法一：每一个完全平方数都可以用首项为1、公差为2的等差数列的连续几项和进行表示，如`1`, `4=1+3`, `9=1+3+5`, `16=1+3+5+7`, ...
```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int sub = 1;
        while (num > 0) {
            num -= sub;
            sub += 2;
        }
        return num == 0;
    }
}
```