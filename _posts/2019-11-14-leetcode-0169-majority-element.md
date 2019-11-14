---
layout: post
title: LeetCode 0169 题解
description: "求众数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[求众数](https://leetcode-cn.com/problems/majority-element/)

### 题解
* 法一（哈希表）：通过hashmap将每个元素及其出现次数储存起来，并返回次数大于$\lfloor \frac{N}{2} \rfloor$的元素。
```java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int maxCount = 0, maxNum = 0;
        for(int i = 0 ; i < nums.length; i++){
            if(!map.containsKey(nums[i]))
                map.put(nums[i], 1);
            else
                map.replace(nums[i], map.get(nums[i])+1);
        }
        for(Integer key : map.keySet()){
            if(map.get(key) > nums.length / 2)
                return key;
        }
        return -1;
    }
}
```
* 法二（Boyer-Moore投票法）：由于众数出现的次数大于$\lfloor \frac{N}{2} \rfloor$，因此若令众数为1，其余数为-1，则总和必定大于0。可将第一个数先设为众数，`count`设为0，并扫描后面的元素，相同时`count`加1，不同时`count`减1，直到`count`变为0时，说明已扫描元素均被抵消，不可能再成为众数；因此将下一元素设为众数，循环此过程，直到扫描完成，留下来的数就是众数。
```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0, candidate = nums[0];
        for(int i = 0; i < nums.length; i++){
            if(count == 0)
                candidate = nums[i];
            if(nums[i] == target)
                count++;
            else
                count--;
        }
        return candidate;
    }
}
```
### 思考
虽然两种方法的时间复杂度都是$O(N)$，但在提交时，投票法明显要比哈希表快。