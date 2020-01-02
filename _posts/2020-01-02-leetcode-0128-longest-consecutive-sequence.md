---
layout: post
title: LeetCode 0128 题解
description: "最长连续序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

### 题解
1. 法一（排序）：对数组排序后，通过查看相邻元素是否为连续来更新最长连续序列的长度。
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums.length == 0)
            return 0;
        Arrays.sort(nums);
        int current = 1, max = 1;
        for(int i = 1; i < nums.length; i++){
            //元素不同时才能进入循环，否则遇到重复元素时会出错。
            if(nums[i] != nums[i-1]){
                if(nums[i] != nums[i-1] + 1)
                    current = 1;
                else{
                    current++;
                    max = Math.max(max, current);
                }
            }
        }
        return max;
    }
}
```
2. 法二（集合）：利用集合储存数组中所有元素，随后遍历数组以查看，对于每一个元素`n`，`n-1`是否存在，不存在说明该元素为某一序列的起始值，则通过这一起始值进入循环依次寻找后面的元素是否存在，并更新最大长度。（暴力法是将每一个元素当成起始值并遍历数组找到其对应的序列，而这种方法是对暴力法的优化，先对真正起始值进行判断再找，减小了循环次数。）
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        int max = 0;
        for(int n : nums)
            set.add(n);
        for(int n : nums){
            if(!set.contains(n - 1)){
                int cur = n;
                int length = 1;
                while(set.contains(cur + 1)){
                    length++;
                    cur++;
                }
                max = Math.max(max, length);
            }
        }
        return max;
    }
}
```
3. 法三（哈希表）：在已出现元素组成的连续序列的起始元素和末尾元素记录该序列长度，每遍历一个元素，利用该元素的前后两元素计算该元素所在序列长度，并更新最大长度，同时对起始元素与末尾元素所记录的值进行更新。
```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int max = 0;
        for(int n : nums){
            // 遇到重复元素需要跳过
            if(map.containsKey(n))
                continue;
            int left = map.getOrDefault(n - 1, 0);
            int right = map.getOrDefault(n + 1, 0);
            // 序列长度
            int length = left + right + 1;
            // 更新序列左右边界的长度
            map.put(n - left, length);
            map.put(n + right, length);
            map.put(n, length);
            max = Math.max(max, length);
        }
        return max;
    }
}
```