---
layout: post
title: LeetCode 0066 题解
description: "加一"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[加一](https://leetcode-cn.com/problems/plus-one/)

### 思路
在末位加一，若进位，则此位为0，上一位加1后返回；若所有数字均为9，则新建数组，并将首位设为1，返回。（这个方法非常巧妙）

### 题解

```java
class Solution {
    public int[] plusOne(int[] digits) {
        for(int i = digits.length - 1; i >= 0; i--) {
        	digits[i]++;
        	digits[i] %= 10;
        	if(digits[i] != 0) return digits;
        }
        int[] nums = new int[digits.length + 1];
        nums[0] = 1;
        return nums;
    }
}
```