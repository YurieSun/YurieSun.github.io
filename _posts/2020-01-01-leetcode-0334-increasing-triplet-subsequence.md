---
layout: post
title: LeetCode 0334 题解
description: "递增的三元子序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[递增的三元子序列](https://leetcode-cn.com/problems/increasing-triplet-subsequence/)

### 题解
1. 法一（暴力法）：对于每一个元素，分别找到在它左边的最小值和右边的最大值，若这三个值满足递增条件则返回，否则继续搜索，直至结束。
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums.length < 3)
            return false;
        for(int i = 1; i < nums.length; i++){
            int min = nums[0], max = nums[nums.length - 1];
            for(int j = 0; j < i; j++)
                min = Math.min(min, nums[j]);
            for(int j = nums.length - 1; j > i; j--)
                max = Math.max(max, nums[j]);
            if(nums[i] > min && nums[i] < max)
                return true;
        }
        return false;
    }
}
```
2. 法二：遍历数组，更新最小值`first`和次小值`second`，若在后面找到了`second`大的元素，则说明存在，否则不存在。（这里要注意，程序结束时的`first`和`second`并不是数组中真正的递增三元子序列中的最小值和次小值，如数组[2,3,1,4,0]中的三元子序列应是[2,3,4]，而程序结束时`first=1`，`second=3`。但该程序仍是正确的，因为若`second`更新了就表明在它前面出现了比它小的元素，那么只要在后面出现了比`second`大的元素，就说明存在递增子序列。）
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums.length < 3)
            return false;
        int first = Integer.MAX_VALUE, second = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] <= first)
                first = nums[i];
            else if(nums[i] <= second)
                second = nums[i];
            else
                return true;
        }
        return false;
    }
}
```
3. 法三：法一中对每个元素都进行了遍历以找到左边的最小值与右边的最大值，那么可以采用两个数组分别存储这两个值。
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums.length < 3)
            return false;
        int[] left = new int[nums.length];
        left[0] = nums[0];
        int[] right = new int[nums.length];
        right[nums.length - 1] = nums[nums.length - 1];
        for(int i = 1; i < nums.length; i++){
            left[i] = Math.min(left[i - 1], nums[i]);
        }
        for(int i = nums.length - 2; i >= 0; i--){
            right[i] = Math.max(right[i + 1], nums[i]);
        }
        for(int i = 0; i < nums.length; i++)
            if(nums[i] > left[i] && nums[i] < right[i])
                return true;
        return false;
    }
}
```