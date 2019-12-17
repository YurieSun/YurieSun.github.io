---
layout: post
title: LeetCode 0229 题解
description: "求众数 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[求众数 II](https://leetcode-cn.com/problems/majority-element-ii/)

### 题解
1. 法一：用哈希表存储各元素出现次数，之后遍历哈希表找出符合条件的元素。
```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> list = new ArrayList<>();
        if(nums == null || nums.length == 0)
            return list;
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0)+1);
        }
        for(Integer key : map.keySet()){
            if(map.get(key) > nums.length/3)
                list.add(key);
        }
        return list;
    }
}
```
2. 法二（Boyer-Moore投票法）：出现次数超过$\lfloor \frac{n}{3} \rfloor$的元素最多只有两个，因此，遍历数组用投票法找到两个候选元素；之后再次遍历数组，检验其次数是否大于超过$\lfloor \frac{n}{3} \rfloor$。
```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> list = new ArrayList<>();
        if(nums == null || nums.length == 0)
            return list;
        int candidateA = nums[0], countA = 0;
        int candidateB = nums[0], countB = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == candidateA)
                countA++;
            else if(nums[i] == candidateB)
                countB++;
            else if(countA == 0){
                candidateA = nums[i];
                countA++;
            }
                
            else if(countB == 0){
                candidateB = nums[i];
                countB++;
            }
            else{
                countA--;
                countB--;
            }
        }
        countA = 0;
        countB = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == candidateA)
                countA++;
            else if(nums[i] == candidateB)
                countB++;
        }
        if(countA > nums.length/3) 
            list.add(candidateA);
        if(countB > nums.length/3)
            list.add(candidateB);
        return list;
    }
}
```