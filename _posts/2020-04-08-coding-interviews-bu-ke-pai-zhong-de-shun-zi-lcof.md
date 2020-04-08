---
layout: post
title: 剑指Offer 61 题解
description: "扑克牌中的顺子"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

### 题解
1. 法一（排序）：排序后遇到`0`则跳过，遇到相同的数直接返回`false`；并且要记录除`0`外的相邻数之差的和，如果这个和小于5说明时顺子，否则不是。这里注意如果判断的是当前数是否为`0`，则计算差值时要用当前数及其下一个数，否则会将当前数和`0`的差值也记录起来，导致判断错误。
```java
class Solution {
    public boolean isStraight(int[] nums) {
        Arrays.sort(nums);
        int sub = 0;
        for (int i = 0; i < 4; i++) {
            if (nums[i] == 0)
                continue;
            if (nums[i] == nums[i + 1])
                return false;
            sub += nums[i + 1] - nums[i];
        }
        return sub < 5;
    }
}
```
2. 法二（不排序）：遇到`0`跳过，使用一个数组记录每个数字出现的次数，出现两次则直接返回`false`；同时记录数组最大值`max`和最小值`min`，如果要组成5张牌的顺子，则最大值和最小值之间的距离`max-min+1`需要小于等于5（若正好等于5，一是没有大小王，显然是顺子；二是有大小王，有则插到中间即可。若小于5，则插到两边即可）。
```java
class Solution {
    public boolean isStraight(int[] nums) {
        int[] cnt = new int[14];
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0)
                continue;
            cnt[nums[i]]++;
            if (cnt[nums[i]] == 2)
                return false;
            max = Math.max(max, nums[i]);
            min = Math.min(min, nums[i]);
        }
        return max - min + 1 <= 5;
    }
}
```