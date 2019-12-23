---
layout: post
title: LeetCode 0220 题解
description: "存在重复元素 III"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[存在重复元素 III](https://leetcode-cn.com/problems/contains-duplicate-iii/)

### 题解
1. 法一（BST）：维护一个长度为`k`的`TreeSet`，同时，遍历数组，找到比当前元素大的最小元素`c`和比当前元素小的最大元素`f`，并检验是否满足条件。（测试用例中总会出现溢出问题，故将`int`转换为`long`。）
```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        TreeSet<Long> set = new TreeSet<>();
        for(int i = 0; i < nums.length; i++){
            Long c = set.ceiling((long)nums[i]);
            if(c != null && c - nums[i] <= t)
                return true;
            Long f = set.floor((long)nums[i]);
            if(f != null && nums[i] - f <= t)
                return true;
            set.add((long)nums[i]);
            if(set.size() > k)
                set.remove((long)nums[i-k]);
        }
        return false;
    }
}
```
2. 法二（桶）：将各元素放到桶里面去，要注意的有：(1)桶的长度选为`t+1`，这样与桶边界相差`t`的元素能和它放在一个桶里，如[0,t]，与`0`相差t的元素`t`在一个桶里，若长度设为`t`，区间变为[0,t-1]，就不在一个桶里；(2)数组元素存在负数，在做除法时会有问题，如当t=3时,-2和2都在[0,3]这个桶中，显然不应该，因此可以将`Integer.MIN_VALUE`作为起始点，这样所有数都是正数，那么在将数放入桶中时就是正确的。随后检查每个元素所在的桶及其相邻的桶，并保持元素总个数不超过`k`。
```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if(k < 1 || t < 0) return false;
        Map<Long, Long> map = new HashMap<>();
        for(int i = 0; i < nums.length; i++){
            long renum = (long)nums[i] - Integer.MIN_VALUE;
            long bucket = renum / ((long)t + 1);
            if(map.containsKey(bucket) ||
               map.containsKey(bucket - 1) && renum - map.get(bucket - 1) <= t ||
               map.containsKey(bucket + 1) && map.get(bucket + 1) - renum <= t)
                return true;
            map.put(bucket, renum);
            if(map.size() > k)
                map.remove(((long)nums[i - k] - Integer.MIN_VALUE) / ((long)t + 1));
        }
        return false;
    }
}
```