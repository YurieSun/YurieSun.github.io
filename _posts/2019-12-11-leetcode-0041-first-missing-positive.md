---
layout: post
title: LeetCode 0041 题解
description: "缺失的第一个正数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)

### 题解
1. 法一（抽屉原理）：缺失的第一个正数，最小为1，最大为数组长度`len`加1，因此只看位于这个范围内的数。则采用抽屉原理，遍历数组，若该数位于$1$到$len$的范围内，则将其放在它应该在的位置上（对于数字`i`，应该将其放在`nums[i-1]`上）；不在这个范围内的数可忽略，不进行交换。再次遍历数组，找到数字与位置不对应的元素，即可找到缺失的第一个正数；若均对应，则缺失的数为$len+1$。
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int len = nums.length;
        // 虽然看上去是两重循环，但每个元素经过1次就能放到正确的位置，其时间复杂度仍为O(n)。
        for(int i = 0; i < len; i++)
            while(nums[i] >= 1 && nums[i] <= len && nums[nums[i]-1] != nums[i]){
                int temp = nums[nums[i]-1];
                nums[nums[i]-1] = nums[i];
                nums[i] = temp;
            }
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != i+1)
                return i+1;
        }
        return len+1;
    }
}
```
* 注意：这种方法在出现重复元素的情况下也适用。
2. 法二：由于只需要看位于$1$到$len$的范围内的数，因此可用正负号来表示元素是否出现。在此之前，要先处理不在该范围的数，遍历数组将其设为1，为了消除原本不含1的影响，需先遍历数组，若不包含1，则返回1；之后便可遍历数组，将不在范围内的数设为1。随后，再次遍历数组，若数字`i`出现，则在`nums[i]`前加上负号。处理完毕，找到第一个出现的正数，其对应下标`i`再加1即为缺失数字。同样，若均为负数，则缺失的数为$len+1$。
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int len = nums.length;
        boolean flag = false;
        for(int x : nums)
            if(x == 1){
                flag = true;
                break;
            }
        if(!flag) return 1;
        if(len == 1) return 2;
        for(int i = 0; i < len; i++)
            if(nums[i] <= 0 || nums[i] > len)
                nums[i] = 1;
        for(int i = 0; i < len; i++){
            int a = Math.abs(nums[i]);
            nums[a-1] = -Math.abs(nums[a-1]);
        }

        for(int i = 0; i< len; i++)
            if(nums[i] > 0)
                return i+1;
        return len+1;  
    }
}
```

* 注意：该方法比较麻烦，主要学习在同一个数组既能表示元素又能表示元素是否出现的思想。