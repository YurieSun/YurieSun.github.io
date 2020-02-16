---
layout: post
title: LeetCode 0594 题解
description: "最长和谐子序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

### 题解
1. 法一（哈希表）：用哈希表存储各元素的出现次数。遍历哈希表，更新和谐子序列的最大长度。
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int length = nums.length;
        int[] res = new int[length];
        for(int i = 0; i < length; i++)
            res[i] = -1;
        for(int i = 0; i < length; i++)
            for(int j = i + 1; j < nums.length + i; j++){
                if(nums[j % length] > nums[i]){
                    res[i] = nums[j % length];
                    break;
                }    
            }
        return res;
    }
}
```
* 在扫描数组并建立哈希表的同时，更新最大长度。
```java
class Solution {
    public int findLHS(int[] nums) {
        int max = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums){
            int cnt = map.getOrDefault(i, 0) + 1;
            map.put(i, cnt);
            if(map.containsKey(i + 1))
                max = Math.max(max, cnt + map.get(i + 1));
            if(map.containsKey(i - 1))
                max = Math.max(max, cnt + map.get(i - 1));
        }
        return max;
    }
}
```