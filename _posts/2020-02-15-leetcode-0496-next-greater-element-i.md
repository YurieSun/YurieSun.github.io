---
layout: post
title: LeetCode 0496 题解
description: "下一个更大元素 I"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[下一个更大元素 I](https://leetcode-cn.com/problems/next-greater-element-i/)

### 题解
1. 法一（二重循环）：先遍历`nums2`，找到每个元素之后的第一个出现的较大值并存入哈希表。然后遍历`nums2`，并从哈希表取值即可。
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int length = nums1.length;
        int[] res = new int[length];
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums2.length; i++){
            for(int j = i + 1; j < nums2.length; j++){
                if(nums2[j] > nums2[i]){
                    map.put(nums2[i], nums2[j]);
                    break;
                }
            }
        } 
        for(int i = 0; i < length; i++)
            res[i] = map.getOrDefault(nums1[i], -1);       
        return res;
    }
}
```
2. 法二（栈）：与法一的方法相同，只是通过建立单调递减栈来找到每个元素后出现的第一个较大值。
```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int length = nums1.length;
        int[] res = new int[length];
        Map<Integer, Integer> map = new HashMap<>();
        Stack<Integer> stack = new Stack<>();
        for(int i = 0; i < nums2.length; i++){
            while(!stack.isEmpty() && nums2[i] > stack.peek())
                map.put(stack.pop(), nums2[i]);
            stack.push(nums2[i]);
        } 
        for(int i = 0; i < length; i++)
            res[i] = map.getOrDefault(nums1[i], -1);       
        return res;
    }
}
```
* 以上两种方法都和 [每日温度](https://leetcode-cn.com/problems/daily-temperatures/) 这道题是类似的。