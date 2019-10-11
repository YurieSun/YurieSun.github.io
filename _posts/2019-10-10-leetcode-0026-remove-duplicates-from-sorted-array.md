---
layout: post
title: LeetCode 0026 题解
description: "删除排序数组中的重复项"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

### 思路
双指针法：一个指针$i$指向无重复元素，另一个指针$j$依次遍历每个元素。当`nums[i] == nums[j]`时，$j$进行递增；一旦不相等时则将$j$的当前元素复制到$i$后面的位置上。

### 题解

```java
public class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums.length == 0) return 0;
        int i = 0;
        for(int j = 1; j < nums.length; j++){
            if(nusm[i] != nums[j]){
                i++;
                nums[i] = nums[j];
            }
        }
        return i+1;
    }
}
```
### 思考
学习双指针法。