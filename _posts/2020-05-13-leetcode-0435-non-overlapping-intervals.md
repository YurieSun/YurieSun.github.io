---
layout: post
title: LeetCode 0435 题解
description: "无重叠区间"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

### 题解
1. 法一（贪心）：将区间根据结束位置进行排序，再遍历区间，找到不重叠的区间个数，则需要移除的区间等于区间总数减去不重叠的区间个数。
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0)
            return 0;
        Arrays.sort(intervals, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[1] - b[1];
            }
        });
        int cnt = 1;
        int end = intervals[0][1];
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < end)
                continue;
            cnt++;
            end = intervals[i][1];
        }
        return intervals.length - cnt;
    }
}
```