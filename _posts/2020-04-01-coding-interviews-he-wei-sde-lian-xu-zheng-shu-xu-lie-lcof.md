---
layout: post
title: 剑指Offer 57 - II 题解
description: "和为s的连续正数序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

### 题解
1. 法一（滑动窗口）：这里的窗口为左闭右开区间。
```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        List<int[]> list = new ArrayList<>();
        int i = 1, j = 1;
        int sum = 0;
        while (i <= target / 2) {
            if (sum == target) {
                int[] temp = new int[j - i];
                for (int index = 0; index < j - i; index++)
                    temp[index] = index + i;
                list.add(temp);
                sum -= i;
                i++;
            } else if (sum > target) {
                sum -= i;
                i++;
            } else {
                sum += j;
                j++;
            }
        }
        return list.toArray(new int[list.size()][]);
    }
}
```