---
layout: post
title: LeetCode 0704 题解
description: "二分查找"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[二分查找](https://leetcode-cn.com/problems/binary-search/)

### 思路
二分查找：练习二分查找通用模板“[十分好用的二分查找法模板](https://leetcode-cn.com/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)”。

### 题解
```java
class Solution {
    public int search(int[] nums, int target) {
        int N = nums.length;
        int left = 0, right = N - 1, mid;
        while(left < right){
            mid = (left + right) >>> 1;
            if(target > nums[mid])
                left = mid + 1;
            else
                right = mid;
        }
        if(nums[left] == target) return left;
        else return -1;
    }
}
```
* 更简洁的写法
```java
class Solution {
    public int search(int[] nums, int target) {
        int N = nums.length;
        int left = 0, right = N - 1, mid;
        while(left <= right){
            mid = (left + right) >>> 1;
            if(target == nums[mid])
                return mid;
            else if(target > nums[mid])
                left = mid + 1;
            else
                right = mid - 1;
        }
        return -1;
    }
}
```