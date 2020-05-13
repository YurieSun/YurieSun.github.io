---
layout: post
title: LeetCode 0452 题解
description: "用最少数量的箭引爆气球"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

### 题解
1. 法一（贪心）：和 [无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/) 一样，区别在于两个区间的起点和重点相同是算作重叠区间的，如[1, 2]和[2, 3]。
```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points == null || points.length == 0)
            return 0;
        Arrays.sort(points, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[1] - b[1];
            }
        });
        int end = points[0][1];
        int cnt = 1;
        for (int i = 1; i < points.length; i++) {
            // 如何定义重叠区间，决定if中是否要取等号。
            if (points[i][0] <= end)
                continue;
            cnt++;
            end = points[i][1];
        }
        return cnt;
    }
}
```