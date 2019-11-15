---
layout: post
title: LeetCode 0189 题解
description: "旋转数组"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[旋转数组](https://leetcode-cn.com/problems/rotate-array/)

### 题解
* 法一：创建新数组来储存旋转后的元素，并通过取余的方式使得数组可回转。
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n];
        for(int i = 0; i < n; i++)
            ans[(i+k)%n] = nums[i];
        for(int i = 0; i < n; i++)
            nums[i] = ans[i];
    }
}
```
* 法二：每次向右移动一位，并移动k次。
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        while(k > 0){
            int temp = nums[n-1];
            for(int i = n-1; i >0; i--)
                nums[i] = nums[i-1];
            nums[0] = temp;
            k--;
        }   
    }
}
```
* 法三（反转法）：先将整个数组进行反转，随后反转前$k$个元素，再反转后$n-k$个元素。
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k %= n; //当k等于n时，反转后的数组与原数组相同；因此当k>n时，反转的结果与k%n相同。
        reverse(nums, 0, n-1);
        reverse(nums, 0, k-1);
        reverse(nums, k, n-1);
    }
    public void reverse(int[] nums, int lo, int hi){
        while(lo < hi) {
        	int temp = nums[lo];
        	nums[lo] = nums[hi];
        	nums[hi] = temp;
        	lo++;
        	hi--;
        }
    }
}
```
* 法四（环状交换法）：从第一个元素开始，每隔$k$步取一个元素（将数组看成可回转）直到回到起始元素，则形成一个环，并向右进行环状交换，直到$n$个元素均进行了交换，则完成；下一次的环状交换起始元素为上一次起始元素的下一个元素。
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int count = 0, n = nums.length;
        k %= n;
        for(int start = 0; count < n; start++){
        // 这里采用count控制循环结束
            int current = start;
            int pre = nums[start];
            do{
                int next = (current+k)%n;
                int temp = nums[next];
                nums[next] = pre;
                pre = temp;
                current = next;
                count++;
            }while(start != current);
        }
    }
}
```