---
layout: post
title: LeetCode 0406 题解
description: "根据身高重建队列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

### 题解
1. 法一（贪心）：对二维数组`people[h][k]`进行排序，排序规则为先按`h`降序，若`h`相等，则按`k`升序。再遍历`people`，根据`k`插到对应的位置。这么做相当于先安排较高的人的位置，在插入身高较矮的人的时候，根据`k`进行插入一定可以保证在这个人之前会有对应数量的比他高的人。
```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        if (people == null || people.length == 0 || people[0].length == 0)
            return new int[0][0];
        Arrays.sort(people, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                if (a[0] == b[0])
                    return a[1] - b[1];
                return b[0] - a[0];
            }
        });
        List<int[]> list = new ArrayList<>();
        for (int[] p : people) {
            list.add(p[1], p);
        }
        return list.toArray(new int[list.size()][2]);
    }
}
```