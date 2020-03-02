---
layout: post
title: LeetCode 0191 题解
description: "位1的个数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)

### 题解
1. 法一：对输入的每一位使用对应的掩码，以统计各位上1的个数。
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int cnt = 0;
        int mask = 1;
        for(int i = 0; i < 32; i++){
            if((n & mask) != 0)
                cnt++;
            mask <<= 1;
        }
        return cnt;
    }
}
```
* 或者将`n`进行右移，不移动掩码。
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int cnt = 0;
        for(int i = 0; i < 32; i++){
            cnt += (n & 1);
            n >>= 1;
        }
        return cnt;
    }
}
```
2. 法二：对于任意数`n`，计算`n&(n-1)`可将`n`的最后一个`1`变为`0`。因此，不断重复该操作，直至`n=0`，并在每一次操作时将`1`的个数加1。
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int cnt = 0;
        while(n != 0){
            cnt++;
            n &= (n - 1);
        }
        return cnt;
    }
}
```