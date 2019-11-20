---
layout: post
title: LeetCode 0448 题解
description: "找到所有数组中消失的数字"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

### 题解
1. 法一（哈希表）：扫描数组后将所有元素存入`set`中，再循环扫描$1$到$n$，将未出现的元素放入`list`中。
```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> list = new ArrayList<>();
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums.length; i++)
            set.add(nums[i]);
        for(int i = 1; i <= nums.length; i++)
            if(!set.contains(i))
                list.add(i);
        return list;
    }
}
```
* 改进：利用数组存放某元素是否存在的结果。
```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> list = new LinkedList<>();
        boolean[] exist = new boolean[nums.length];
        for(int i = 0; i < nums.length; i++)
            exist[nums[i]-1] = true;
        for(int i = 0; i < nums.length; i++)
            if(!exist[i])
                list.add(i+1);
        return list;
    }
}
```
2. 法二（抽屉原理+异或交换）：将数组中的数放到它原本应该在的位置，最后扫描数组，若某位置上放的不是该下标对应的数，则说明这个数是缺失的；同时为了不使用额外空间，使用异或来交换两元素位置。
```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> list = new LinkedList<>();
        for(int i = 0; i < nums.length; i++){
            while(nums[nums[i]-1] != nums[i])
                swap(nums, nums[i]-1, i);
        }
        for(int i = 0; i < nums.length; i++)
            if(nums[i] - 1 != i)
                list.add(i+1);
        return list;
    }
    // 使用异或运算进行元素交换，即若要交换a、b两数，则可通过a=a^b, b=a^b, a=a^b得到。
    public void swap(int[] nums, int i, int j){
        nums[i] = nums[i] ^ nums[j];
        nums[j] = nums[i] ^ nums[j];
        nums[i] = nums[i] ^ nums[j];
    }
}
```
* 注意：抽屉原理与异或交换值得学习。