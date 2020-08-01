---
layout: post
title: LeetCode 0769 题解
description: "最多能完成排序的块"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最多能完成排序的块](https://leetcode-cn.com/problems/max-chunks-to-make-sorted/)

### 题解
1. 法一：按照规则，设分块后的每一块下标范围为[a, b]，则这一块中只能存放[a, b]之间的数，才能满足要求。因此，在遍历数组时，求当前遍历过的元素最大值，如果这个最大值等于当前下标，说明可以分一块。
```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        if (arr == null || arr.length == 0)
            return 0;
        int res = 0;
        int max = 0;
        for (int i = 0; i < arr.length; i++) {
            // 求最大值
            max = Math.max(arr[i], max);
            // 最大值与下标相等，则可以分块。
            if (max == i) {
                res++;
            }
        }
        return res;
    }
}
```