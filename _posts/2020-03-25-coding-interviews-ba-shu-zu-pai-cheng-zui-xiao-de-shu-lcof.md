---
layout: post
title: 剑指Offer 45 题解
description: "把数组排成最小的数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

### 题解
1. 法一：排序时传入一个比较器，若`s1+s2>s2+s11`，则再拼接时应该把`s2`放在前面。
```java
class Solution {
    public String minNumber(int[] nums) {
        if (nums == null || nums.length == 0)
            return "";
        List<String> strList = new ArrayList<>();
        for (int n : nums) {
            strList.add(String.valueOf(n));
        }
        strList.sort((s1, s2) -> (s1 + s2).compareTo(s2 + s1));
        StringBuilder res = new StringBuilder();
        for (String str : strList) {
            res.append(str);
        }
        return res.toString();
    }
}
```