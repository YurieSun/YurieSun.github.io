---
layout: post
title: LeetCode 0673 题解
description: "最长递增子序列的个数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

### 题解
1. 法一（动态规划）：和最长递增子序列做法类似，只不过需要在动态规划的过程中记录出现次数。`dp[i]`表示以`nums[i]`结尾的最长递增子序列的长度；新增数组`count`，`count[i]`表示以`nums[i]`结尾的最长递增子序列的个数。若与`dp[i]`相比，出现了更大的长度，则表明这是第一次出现，则当前长度的出现次数`count[i]`就是`count[j]`；若出现了相同长度，则直接加上即可。同时，在遍历过程中，记录`dp[i]`的最大值，即最长递增子序列的长度。最后遍历`count`，若与最长的长度相等，则将它的个数加到最终结果上。
```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        int[] dp = new int[nums.length];
        int[] count = new int[nums.length];
        Arrays.fill(dp, 1);
        Arrays.fill(count, 1);
        int max = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    int cnt = dp[j] + 1;
                    if (cnt > dp[i]) {
                        dp[i] = cnt;
                        count[i] = count[j];
                    } else if (cnt == dp[i]) {
                        count[i] += count[j];
                    }
                }
            }
            max = Math.max(max, dp[i]);
        }
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            if (dp[i] == max) {
                res += count[i];
            }
        }
        return res;
    }
}
```