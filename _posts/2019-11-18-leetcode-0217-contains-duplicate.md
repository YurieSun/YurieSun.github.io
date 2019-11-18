---
layout: post
title: LeetCode 0217 题解
description: "存在重复元素"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

### 题解
1. 法一（哈希表）：依次扫描数组，并用哈希表存储各元素的出现次数；若是第一次出现，则存入哈希表中，若已出现，则返回，直到扫描结束。
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            if(map.containsKey(nums[i]))
                return true;
            else
                map.put(nums[i], 1);
        }
        return false;
    }
}
```
* 改进：不需要采用`hashmap`键值对的形式来存储出现次数，而是采用`set`中没有重复元素的特性。
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>(nums.length);
        for(int x : nums){
            if(set.contains(x))
                return true;
            set.add(x);
        }
        return false;
    }
}
```
2. 法二（暴力法）：扫描数组，对于每个元素，检查它前面是否有与之相等的元素。
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        for(int i = 0; i < nums.length; i++)
            for(int j = 0; j < i; j++){
                if(nums[i] == nums[j])
                    return true;
            }
        return false;
    }
}
```
3. 法三（排序法）：先将数组排序，再扫描数组，若有相邻元素相等，则返回`true`，否则扫描结束并返回。
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for(int i = 1; i < nums.length; i++)
            if(nums[i] == nums[i-1])
                return true;
        return false;
    }
}
```
* 注意：最好不要轻易修改输入数据，因此若要采用此方法，最好先复制数组进行排序。