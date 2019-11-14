---
layout: post
title: LeetCode 0167 题解
description: "两数之和 II - 输入有序数组"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

### 题解
* 法一：暴力算法进行列举。
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int[] ans = new int[]{0,0};
        for(int i = 0; i < numbers.length; i++)
            for(int j = i+1; j < numbers.length; j++)
                if(numbers[i] + numbers[j] == target){
                    ans[0] = i+1;
                    ans[1] = j+1;
                }   
        return ans;
    }
}
```
* 法二：采用双指针，`i`指向第一个元素，`j`指向第二个元素；若两数之和小于`target`，则增加`i`，若大于则增加`j`；直到找到或遍历完所有元素结束。（利用数组已排序这一特性）
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int i = 0, j = numbers.length-1;
        while(i < j || numbers[i] + numbers[j] == target){
            if(numbers[i] + numbers[j] < target)
                i++;
            else if(numbers[i] + numbers[j] > target)
                j--;
            else
                return new int[]{i+1, j+1};
        }
        return new int[]{0, 0};
    }
}
```