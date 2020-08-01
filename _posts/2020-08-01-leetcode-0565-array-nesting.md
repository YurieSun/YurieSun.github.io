---
layout: post
title: LeetCode 0565 题解
description: "数组嵌套"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数组嵌套](https://leetcode-cn.com/problems/array-nesting/)

### 题解
1. 法一（抽屉原理）：每个元素只能处于一个闭环内，而通过抽屉原理将元素放回它该放的位置，每次将一个元素放好后，就可以形成一个闭环，而这个闭环的元素个数就是交换次数+1.
```java
class Solution {
    public int arrayNesting(int[] nums) {
        int res = 1;
        for (int i = 0; i < nums.length; i++) {
            // 每次循环的初始值设为1，这是因为一个元素所在的闭环至少会有自己。
            int cnt = 1;
            // 通过抽屉原理把当前下标的元素放对位置。
            while (nums[i] != nums[nums[i]]) {
                int temp = nums[nums[i]];
                nums[nums[i]] = nums[i];
                nums[i] = temp;
                cnt++;
            }
            res = Math.max(res, cnt);
        }
        return res;
    }
}
```
2. 法二：每个元素只属于一个闭环，因此根据下标规则访问元素，并将访问过的元素置为-1，一直嵌套访问直到遇到-1，说明闭环结束，就可以统计该闭环的元素个数了。
```java
class Solution {
    public int arrayNesting(int[] nums) {
        int res = 1;
        for (int i = 0; i < nums.length; i++) {
            int cur = 0;
            int idx = i;
            while (nums[idx] != -1) {
                // 先把当前下标存下来，用来修改当前元素的值为-1。接下来将下标修改为下一个要访问的地址。
                int temp = idx;
                idx = nums[idx];
                nums[temp] = -1;
                cur++;
            }
            res = Math.max(res, cur);
        }
        return res;
    }
}
```