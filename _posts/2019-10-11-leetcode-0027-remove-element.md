---
layout: post
title: LeetCode 0027 题解
description: "移除元素"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[移除元素](https://leetcode-cn.com/problems/remove-element/)

### 思路
双指针法：$i$为慢指针，$j$为快指针。若`nums[i] !=nums[j]`，则将`num[j]`复制到$i$的位置，否则$j$递增，忽略不相等元素。

### 题解

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        if(nums.length == 0) return 0;
        int i = 0;
        for (int j = 0; j < nums.length; j++){
            if (nums[j] != val){
                nums[i] = nums[j];
                i++;
            }
        }
        return i;
    }
}
```
* 也可写为：
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int i = 0;
        for(int n :nums)
            if(n != val)
                nums[i++] = n;
        return i;
    }
}
```
* 改进  
若元素很多时，复制元素消耗较多资源和时间，因此采用另一种方案（该方案改变了删除后的元素位置）：  
若`nums[i] == val`，则将最后一个元素复制到$i$的位置上，同时数组长度减1；若不相等则递增$i$；重复该过程直至数组末尾。

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        if(nums.length == 0) return 0;
        int i = 0, n = nums.length;
        while (i < n) {
            if (nums[i] == val){
                nums[i] = nums[n-1];
                n--;
            }
            else
                i++;
        }
        return n;
    }
}
```

### 思考
继续学习双指针法。