---
layout: post
title: LeetCode 0628 题解
description: "三个数的最大乘积"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[三个数的最大乘积](https://leetcode-cn.com/problems/maximum-product-of-three-numbers/)

### 题解
1. 法一：暴力法（超时）

2. 法二（排序法）：对数组进行排序，由于数组中存在负数，因此需要比较【最小两个数与最大数】这三个数的乘积与【最大的三个数】的乘积，返回较大者。（注意：不能根据【最小两个数】的乘积与【第二大和第三大两个数】的乘积决定返回值，因为最大数有可能是负数，因此只能比较三个数乘积的大小）
```java
class Solution {
    public int maximumProduct(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        return Math.max(nums[0]*nums[1]*nums[n-1], nums[n-3]*nums[n-2]*nums[n-1]); 
    }
}
```

3. 法三：遍历数组，找到最小的两个数和最大的三个数，再和上述方法中一样进行判断返回。
```java
class Solution {
    public int maximumProduct(int[] nums) {
        int loFirst = Integer.MAX_VALUE, loSecond = Integer.MAX_VALUE;
        int hiFirst = Integer.MIN_VALUE, hiSecond = Integer.MIN_VALUE, hiThird = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] < loFirst){
                loSecond = loFirst;
                loFirst = nums[i];
            }
            else if(nums[i] < loSecond)
                loSecond = nums[i];
            if(nums[i] > hiFirst){
                hiThird = hiSecond;
                hiSecond = hiFirst;
                hiFirst = nums[i];
            }
            else if(nums[i] > hiSecond){
                hiThird = hiSecond;
                hiSecond = nums[i];
            }
            else if(nums[i] > hiThird)
                hiThird = nums[i];
        }
        return Math.max(loFirst*loSecond*hiFirst, hiThird*hiSecond*hiFirst);
    }
}
```