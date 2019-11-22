---
layout: post
title: LeetCode 0532 题解
description: "数组中的K-diff数对"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数组中的K-diff数对](https://leetcode-cn.com/problems/k-diff-pairs-in-an-array/)

### 题解
1. 法一：在哈希表中存储各元素及其出现次数；随后遍历哈希表，若`k=0`，则统计出现两次以上的元素，反之，记录`i+k`或`i-k`均可。
```java
class Solution {
    public int findPairs(int[] nums, int k) {
        if(k < 0) return 0;
        int count = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            if(!map.containsKey(nums[i]))
                map.put(nums[i], 1);
            else
                map.put(nums[i], map.get(nums[i])+1);
        }
        for(int i : map.keySet()){
            if(k == 0){
                if(map.get(i) >= 2) count++;
            }
            else {
                if(map.containsKey(i+k))
                    count++;
            }
        }
        return count;
    }
}
```

2. 法二（简洁）：使用两个`set`存储数据，其中`saw`存储已经访问过的元素，`diff`存储已发现的K-diff数对中的较小数；遍历完数组后返回`diff`的长度即可。
```java
class Solution {
    public int findPairs(int[] nums, int k) {
        if(k < 0) return 0;
        Set<Integer> saw = new HashSet<>();
        Set<Integer> diff = new HashSet<>();
        for(int i : nums){
            if(saw.contains(i-k))
                diff.add(i-k);
            if(saw.contains(i+k))
                diff.add(i);
            saw.add(i);
        }
        return diff.size();
    }
}
```