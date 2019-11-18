---
layout: post
title: LeetCode 0219 题解
description: "存在重复元素 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)

### 题解
1. 法一（哈希表）：依次扫描数组，并用哈希表存储各元素出现的下标；若是第一次出现，则存入哈希表中，若已出现，则取出上次的下标，符合条件则返回，不符合则更新该元素下标，直到扫描结束。
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            if(map.containsKey(nums[i]) && i - map.get(nums[i]) <= k)
                return true;
            map.put(nums[i], i);
        }
        return false;
    }
}
```
* 改进（滑动窗口法）：根据`set`中没有重复元素的特性，维护一个长度为$k$的`set`。
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        for(int i = 0; i < nums.length; i++){
            if(set.contains(nums[i]))
                return true;
            set.add(nums[i]);
            if(set.size() > k)
                set.remove(nums[i - k]);
        }
        return false;
    }
}
```
2. 法二（暴力法）：也是滑动窗口。扫描数组，对于每个元素，检查它前面$k$个元素中是否有与之相等的元素。
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        for(int i = 0; i < nums.length; i++)
            for(int j = Math.max(i - k, 0); j < i; j++)
                if(nums[i] == nums[j])
                    return true;
        return false;
    }
}
```
3. 法三（BST法）：通过$BST$维护一个长度为$k$的滑动窗口。
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
            Set<Integer> set = new TreeSet<>();
            for (int i = 0; i < nums.length; i++){
                if(set.contains(nums[i]))
                    return true;
                set.add(nums[i]);
                if(set.size() > k)
                    set.remove(nums[i - k]);
            }
            return false;
    }
}
```
* 注意：这个方法与法一的改进是一样的，区别仅在于`set`是通过`HashSet`实现还是`TreeSet`实现。但该方法的运行速度较慢，这是因为`TreeSet`是有序的，它的插入、删除、搜索的时间复杂度均为$O(\lg k)$，而`HashSet`无序，则它的插入、删除、搜索的时间复杂度均为$O(1)$。因此，若实现与顺序无关的算法，采用`HashSet`较好。