---
layout: post
title: 剑指Offer 60 题解
description: "圆圈中最后剩下的数字"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

### 题解
1. 法一：这其实是约瑟夫环问题，这里使用数学推导得到一些关系。已知最后存活的那个人，数组长度为1，其编号也为1。因此如果能得到旧编号与新编号之间的关系，就可以从最后存活的情况向上递推，得到这个人在数组长度为`n`时的编号。首先，寻找在数组长度为`i`的情况下，编号`s`与报数`m`之间的关系（本题中将数组开头的编号设为1）。我们很容易得出 $y=x\%i$ 的图像，同时可以画出`s`与`m`的关系图，发现是由 $y=x\%i$ 右移一个单位并上移一个单位得到的，因此根据图像的平移变换得出 $s=(m-1)\%i+1$ 。接下来，我们寻找旧编号`old`与新编号`new`之间的关系。同样画出它们的关系图，可以发现是由 $y=x\%i$ 左移`s-1`个单位并上移一个单位得到的，因此有 $old=(new+s-1)\%i+1$ 。将 $s=(m-1)\%i+1$ 代入上式化简得 $old=(new+m-1)\%i+1$ 。由这个递推公式得到了旧编号`old`、新编号`new`、报数`m`和数组长度`i`之间的关系；且最后存活的情况为`i=1`，其编号`new=1`，据此就可以得到`i=2`、`i=3`一直到`i=n`的旧编号`old`。由于给定数组是从`0`开始编号为`1`，因此最终应返回编号-1。
```java
class Solution {
    public int lastRemaining(int n, int m) {
        return getAlive(n, m) - 1;
    }
    // 这个递归函数就是递推公式，其中new = getAlive(i-1, m)。
    private int getAlive(int i, int m) {
        if (i == 1)
            return 1;
        return (getAlive(i - 1, m) + m - 1) % i + 1;
    }
}
```
* 改成迭代版本：
```java
class Solution {
    public int lastRemaining(int n, int m) {
        int res = 1;
        int i = 1;
        while (i <= n) {
            res = (res + m - 1) % i + 1;
            i++;
        }
        return res - 1;
    }
}
```