---
layout: post
title: LeetCode 0376 题解
description: "摆动序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

### 题解
1. 法一（动态规划）：定义两个数组，`up[i]`表示以`nums[i]`结尾且`nums[i]`相比它前一个元素是上升的摆动序列长度，`down[i]`表示以`nums[i]`结尾且`nums[i]`相比它前一个元素是下降的摆动序列长度，使用LIS的方法进行更新，最终返回这两个数组中的最大值。
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        Arrays.fill(up, 1);
        Arrays.fill(down, 1);
        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i], down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i], up[j] + 1);
                }
            }
        }
        int maxUp = 0, maxDown = 0;
        for (int i = 0; i < up.length; i++) {
            maxUp = Math.max(maxUp, up[i]);
            maxDown = Math.max(maxDown, down[i]);
        }
        return Math.max(maxUp, maxDown);
    }
}
```
* 改进：由上述状态转移方程可知，数组`up`和`down`均是非递减的，因此一定在数组末尾达到最大值，最后不需要再次遍历数组寻找最大值，直接比较两数组中最后一项即可。
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        Arrays.fill(up, 1);
        Arrays.fill(down, 1);
        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i], down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i], up[j] + 1);
                }
            }
        }
        return Math.max(up[nums.length - 1], down[nums.length - 1]);
    }
}
```
2. 法二（动态规划）：改变两个数组的定义，`up[i]`是前i个元素中，摆动序列以上升元素结尾的最长子序列长度，不一定以`nums[i]`结尾；`down[i]`是前i个元素中，摆动序列以下降元素结尾的最长子序列长度，不一定以`nums[i]`结尾。
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        int[] up = new int[nums.length];
        int[] down = new int[nums.length];
        Arrays.fill(up, 1);
        Arrays.fill(down, 1);
        for (int i = 1; i < nums.length; i++) {
            // 说明上升子序列长度需要增加
            if (nums[i] > nums[i - 1]) {
                up[i] = down[i - 1] + 1;
                down[i] = down[i - 1];
            } 
            // 说明下降子序列长度需要增加
            else if (nums[i] < nums[i - 1]) {
                up[i] = up[i - 1];
                down[i] = up[i - 1] + 1;
            } 
            else {
                up[i] = up[i - 1];
                down[i] = down[i - 1];
            }
        }
        return Math.max(up[nums.length - 1], down[nums.length - 1]);
    }
}
```
* 改进：上述两个数组，均只用到了前一项进行计算，因此可用两个变量进行计算，以降低空间复杂度。
```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums == null || nums.length == 0)
            return 0;
        int up = 1, down = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i - 1]) {
                up = down + 1;
            } else if (nums[i] < nums[i - 1]) {
                down = up + 1;
            }
        }
        return Math.max(up, down);
    }
}
```