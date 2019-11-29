---
layout: post
title: LeetCode 0697 题解
description: "数组的度"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)

### 题解
1. 法一：使用三个哈希表，分别存放数组中元素第一次出现的位置、最后一次出现的位置、出现的次数；找到出现次数最多的元素（可能有多个），并分别计算其中每一个元素第一次出现和最后一次出现的间隔，取其中的最小值。
```java
class Solution {
    public int findShortestSubArray(int[] nums) {
        Map<Integer, Integer> left = new TreeMap<>();
        Map<Integer, Integer> right = new TreeMap<>();
        Map<Integer, Integer> count = new TreeMap<>();
        for(int i = 0; i < nums.length; i++){
            if(!left.containsKey(nums[i]))
                left.put(nums[i], i);
            right.put(nums[i], i);
            count.put(nums[i], count.getOrDefault(nums[i], 0)+1);
        }
        int ans = nums.length;
        int degree = Collections.max(count.values());
        for(int x : nums){
            if(count.get(x) == degree)
                ans = Math.min(ans, right.get(x) - left.get(x) + 1);
        }
        return ans;
    }
}
```
2. 法二：与上述方法思路相同，只是采用三个数组分别存放哈希表中的内容。
```java
class Solution {
    public int findShortestSubArray(int[] nums) {
        if(nums == null) return 0;
        int max = Integer.MIN_VALUE, cnt = 0, ans = nums.length;
        for(int x : nums)
            max = Math.max(max, x);
        int[] left = new int[max+1];
        int[] right = new int[max+1];
        int[] count = new int[max+1];
        for(int i = 0; i < nums.length; i++){
            if(count[nums[i]] == 0)
                left[nums[i]] = i;
            right[nums[i]] = i;
            count[nums[i]]++;
            cnt = Math.max(cnt, count[nums[i]]);
        }
        for(int i = 0; i < count.length; i++){
            if(count[i] == cnt)
                ans = Math.min(ans, right[i] - left[i] + 1);
        }
        return ans;
    }
}
```
* 注意：法二的运行速度比法一快，但当数组中不相同元素的个数较少，则法二较浪费空间。