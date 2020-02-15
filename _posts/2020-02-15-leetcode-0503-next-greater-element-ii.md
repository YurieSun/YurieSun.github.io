---
layout: post
title: LeetCode 0503 题解
description: "下一个更大元素 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

### 题解
1. 法一（二重循环）：找到每个元素之后的较大值的位置，注意若到达数组末尾还未找到则需从数组开头继续找。
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
2. 法二（栈）：通过单调栈来找。对于这题，需要先找元素右边是否有更大的，没有则继续找元素左边。相当于遍历一个将元素左边元素复制到数组末尾的新数组，而通过控制`i`的遍历次数和`%`的运算，不需要构建新数组即可完成。
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int length = nums.length;
        int[] res = new int[length];
        Stack<Integer> stack = new Stack<>();
        for(int i = 0; i < length; i++)
            res[i] = -1;
        for(int i = 0; i < 2 * nums.length; i++){
            int num = nums[i % length];
            while(!stack.isEmpty() && num > nums[stack.peek()])
                res[stack.pop()] = num;
            stack.push(i % length);
        }
        return res;
    }
}
```