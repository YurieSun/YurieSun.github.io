---
layout: post
title: LeetCode 0283 题解
description: "移动零"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[移动零](https://leetcode-cn.com/problems/move-zeroes/)

### 题解
1. 法一（双指针法）：当快指针`j`扫描到不为0的元素时，将其赋给慢指针`i`所在位置，同时递增`i`；快指针`j`将数组扫描完成后，将`i`后面的元素全部赋为0。
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0;
        for(int j = 0; j < nums.length; j++){
            if(nums[j] != 0){
                nums[i] = nums[j];
                i++;
            }
        }
        for(int j = i; j < nums.length; j++)
            nums[j] = 0;
    }
}
```
* 改进：上述方法在$0$较多的情况时，最后需要填充很多$0$。因此，当快指针`j`扫描到不为0的元素时，可将其与慢指针`i`所在元素交换置，同时递增`i`，直至扫描完成。
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0;
        for(int i = 0, j = 0; j < nums.length; j++){
            if(nums[j] != 0){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
            }
        }
    }
}
```
* 注意：在第一个方法中，`i`必须在`for`循环外定义，使得第二次循环的初值还存在；改进方法中，`i`可以在循环中定义。