---
layout: post
title: LeetCode 0067 题解
description: "二进制求和"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二进制求和](https://leetcode-cn.com/problems/add-binary/)

### 思路
从低位开始相加，若位数不够则补0，直到两个字符串均扫描完；同时记录每一位相加是否需要进位。

### 题解

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder ans = new StringBuilder();
    	int ca = 0;
        for(int i = a.length()-1, j = b.length()-1; i >= 0 || j >= 0; i--,j--) {
        	int sum = ca;
        	sum += (i >=0 ? a.charAt(i)-'0' : 0);
        	sum += (j >=0 ? b.charAt(j)-'0' : 0);
        	ans.append(sum % 2);
        	ca = sum / 2;
        }
        ans.append(ca == 1 ? ca : "");
        return ans.reverse().toString();
    }
}
```

### 思考
其他进制的加法可参考该做法，如十进制加法只需把其中的2换成10。