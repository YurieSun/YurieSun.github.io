---
layout: post
title: LeetCode 0414 题解
description: "第三大的数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[第三大的数](https://leetcode-cn.com/problems/third-maximum-number/)

### 题解
1. 法一（哈希表）：维护一个长度为$3$的`treeset`，以保证该集合有顺序；扫描数组，若`treeset`的长度大于$3$，则删去其中最小的数；当扫描结束后，若长度等于$3$，则返回其中最小的数，若长度小于$3$，则返回其中最大的数。
```java
class Solution {
    public int thirdMax(int[] nums) {
        TreeSet<Integer> set = new TreeSet<>(); 
        //注意：左边不能定义为Set接口，否则会产生编译错误，因为Set中没有first()和last()方法。
        for(int i = 0; i < nums.length; i++){
            set.add(nums[i]);
            if(set.size() > 3)
                set.remove(set.first());
        }
        if(set.size() <  3)
            return set.last();
        return set.first();
    }
}
```
2. 法二（三变量）：用三个变量分别存储第一大、第二大和第三大的元素，在扫描数组时进行更新；扫描完成后，若没有第三大的元素，则返回最大值，反之返回第三大的数。
```java
class Solution {
    public int thirdMax(int[] nums) {
        if(nums.length == 0) return -1;
        int one = nums[0];
        long two = Long.MIN_VALUE,three = Long.MIN_VALUE;
        for(int i = 0; i< nums.length; i++){
            if(nums[i] == one || nums[i] == two || nums[i] == three)
                continue;
            if(nums[i] > one){
                three = two;
                two = one;
                one = nums[i];
            }  
            else if(nums[i] > two){
                three =two;
                two = nums[i];
            }    
            else if(nums[i] > three)
                three = nums[i];
        }
        if(three == Long.MIN_VALUE)
            return one;
        return (int)three;
    }
}
```
* 注意：这三个变量的初始赋值；LeetCode的测试用例中，有一组的一个值正好是`int`的最小值，所以若赋初值时采用`int`的最小值，在这个用例中的返回值是错的，因此采用`long`的最小值进行赋值。