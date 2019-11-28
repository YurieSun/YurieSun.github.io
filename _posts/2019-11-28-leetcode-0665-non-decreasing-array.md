---
layout: post
title: LeetCode 0665 题解
description: "非递减数列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/)

### 题解
1. 法一：扫描数组，当碰到单调递减的序列（即`nums[i-1] > nums[i]`）时，分析以下4种情况：(1)该递减序列出现在数组的开头，则将第一个元素更换即可；(2)该递减序列出现在数组的末尾，则同样更换最后一个元素即可；(3)否则，该序列出现在数组中间，还应根据`nums[i]`与`nums[i-2]`的大小分为以下情况(i)若`nums[i] > nums[i-2]`（如：-1, 4, 2, 3），则需要更改的是递减序列的第一个数`nums[i-1]`，(ii)若`nums[i] <= nums[i-2]`（如：3, 4, 2, 5），则需要更改的是递减序列的第一个数`nums[i-1]`。以上需要更换成的数字并不是唯一的，所有取的是能够换成的最小数字，以保证非递减。最后，每次出现非递减的情况都需要更新次数，若次数大于0，则不可能通过一次交换满足题意。
```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        int count = 0;
        for(int i = 1; i < nums.length; i++){
            if(nums[i-1] > nums[i]){
                if((i-1) == 0)
                    nums[i-1] = nums[i];
                else if(i == nums.length-1)
                    nums[i] = nums[i-1];
                else if(nums[i] >= nums[i-2])
                    nums[i-1] = nums[i];
                else if(nums[i] < nums[i-2])
                    nums[i] = nums[i-1];
                count++;
            }
        }
        return count <= 1;
    }
}
```
* 改进：当递减序列出现在数组首尾时，需要更改的是第一个或最后一个元素，而它们并不会影响数组中间的递减序列的判定，因此对于这两种情况，可以不去更换元素，即仅需考虑在数组中间的递减序列的元素更换。同时，在循环条件中增加次数的判定，可提前结束循环。
```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        int count = 0;
        for(int i = 1; i < nums.length && count < 2; i++){
            if(nums[i-1] <= nums[i])
                continue; 
            if(i - 2 >= 0 && nums[i] < nums[i-2])
                nums[i] = nums[i-1];
            else 
            /*这里包括两种情况，一是i=1，二是nums[i] >= nums[i-2]，也就是上述情况(1)和(3-i)；
              因此上面的if条件中不能改成if(i - 2 >= 0 && nums[i] > nums[i-2])，
              这样的话，else的情况就没法统一写成nums[i-1] = nums[i]。
            */
                nums[i-1] = nums[i];
            count++;
        }
        return count <= 1;
    }
}
```