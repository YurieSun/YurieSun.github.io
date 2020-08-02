---
layout: post
title: LeetCode 0338 题解
description: "比特位计数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[比特位计数](https://leetcode-cn.com/problems/counting-bits/)

### 题解
1. 法一：对于每个数字，都判断一下该数有多少个1。
```java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        for (int i = 0; i < res.length; i++) {
            res[i] = count(i);
        }
        return res;
    }
    // 判断一个数字中有几个1
    public int count(int num) {
        int res = 0;
        while (num != 0) {
            res++;
            num &= (num - 1);
        }
        return res;
    }
}
```
2. 法二：可以考虑用前面计算的结果得到当前结果。对于奇数，一定比它前一个数（偶数）多一个1，多出来的就是最低位的那个1；对于偶数，和它除以2的那个数中1的个数是一样的，因为除以2相当于右移一位，把末尾的0去掉了，对1的个数没有影响。因此，可以利用前面的结果避免对每个数都进行扫描。
```java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        for (int i = 0; i < res.length; i++) {
            if (i % 2 == 0) {
                res[i] = res[i / 2];
            } else {
                res[i] = res[i - 1] + 1;
            }
        }
        return res;
    }
}
```
* 另一种思路：对于偶数，和它除以2的那个数中1的个数是一样的；对于奇数，比它除以2的那个数中1的个数多1。（通过位运算优化）
```java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        for (int i = 0; i < res.length; i++) {
            // 右边等价于res[i/2]+(i%2)，这里通过位运算优化了。
            res[i] = res[i >> 1] + (i & 1);
        }
        return res;
    }
}
```
* 另一种思路，把当前数最低位的1变成0，则变换后的数一定比当前数小，之前肯定算过，则当前数中1的个数就等于变换后的数中1的个数加1。
```java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        // 这里注意从1开始，否则i-1会变成负数，导致0的结果不对，后面的结果也会不对。
        // 其实其它方法也可以从1开始，因为0不需要计算。
        for (int i = 1; i < res.length; i++) {
            res[i] = res[i & (i - 1)] + 1;
        }
        return res;
    }
}
```