---
layout: post
title: LeetCode 0645 题解
description: "错误的集合"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[错误的集合](https://leetcode-cn.com/problems/set-mismatch/)

### 题解
1. 法一：遍历数组，用哈希表记录元素出现次数；再遍历1到n，找到出现2次和0次的数。
```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }
        int miss = 0, dup = 0;
        for (int i = 1; i <= nums.length; i++) {
            // 先判断该元素是否存在，不存在需要退出，否则map的get()会出现空指针异常。
            if (!map.containsKey(i)) {
                miss = i;
                continue;
            }
            if (map.get(i) == 2) {
                dup = i;
            }
        }
        return new int[]{dup, miss};
    }
}
```
* 改进：可以用数组记录出现次数。
```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int[] count = new int[nums.length + 1];
        for (int n : nums) {
            count[n]++;
        }
        int miss = 0, dup = 0;
        for (int i = 1; i < count.length; i++) {
            if (count[i] == 0) {
                miss = i;
            }
            if (count[i] == 2) {
                dup = i;
            }
        }
        return new int[]{dup, miss};
    }
}
```
2. 法二（位运算）：可以先看 剑指Offer第56题，[数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)的思路，是如何找到两个只出现1次的元素的。本题可以将【1到n的序列】和【nums数组】看成一个合并的数组，则出现2次和出现0次的元素就分别出现了3次和1次，而其余元素均出现了2次。其实进行异或运算后，出现3次和出现1次的效果是一样的，因此可以把这个合并数组中出现3次和1次的两个元素找出来。但在本题中，需要区分这两个元素中哪个在原数组中是出现2次的，因此需要再次遍历数组，统计某一个元素的出现次数即可。
```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int temp = 0;
        // 将两个数组看成一个数组，对所有元素进行异或运算。
        for (int i = 0; i < nums.length; i++) {
            temp ^= (i + 1) ^ nums[i];
        }
        // 找到第一个低位为1的位置
        int mask = temp & (-temp);
        int res1 = 0, res2 = 0;
        // 根据mask把合并的数组分成两组，从而得到出现3次和1次的元素。
        for (int i = 1; i <= nums.length; i++) {
            if ((i & mask) == 0) {
                res1 ^= i;
            } else {
                res2 ^= i;
            }
        }
        for (int n : nums) {
            if ((n & mask) == 0) {
                res1 ^= n;
            } else {
                res2 ^= n;
            }
        }
        // 再次计数，区分出现2次和0次的元素。
        int count = 0;
        for (int n : nums) {
            if (res1 == n) {
                count++;
            }
        }
        if (count == 0) {
            return new int[]{res2, res1};
        }
        return new int[]{res1, res2};
    }
}
```
3. 法三（抽屉原理）：把元素放到它应该在的地方，即数字i应该放在nums[i-1]上；再遍历该数组，若不满足这个规律的，则下标为缺失的数字，该下标所在位置的元素为重复元素。这是时间复杂度为$O(n)$的排序，比一般排序的时间复杂度$O(n\lg n)$要低，当数组是从1到n的连续序列时可以使用这个方法进行排序。
```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            // 一定需要先替换nums[nums[i] - 1]，若先替换nums[i]，会导致nums[nums[i] - 1]的下标发生变化。
            while (nums[nums[i] - 1] != nums[i]) {
                int temp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                return new int[]{nums[i], i + 1};
            }
        }
        return new int[0];
    }
}
```