---
layout: post
title: LeetCode 0137 题解
description: "只出现一次的数字 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[只出现一次的数字 II](https://leetcode-cn.com/problems/single-number-ii/)

### 题解
1. 法一：对于每个数字上的某一位来说，如果这个数字出现了`3`次，则该位上出现`1`的次数为`3`的倍数即`3n`；如果只出现了`1`次，则该位上出现`1`的次数为`3n+1`。因此，对于32位整数，可以对每一位上出现`1`的次数进行统计，从而知道只出现一次的数字对应的位数。
```java
class Solution {
    public int singleNumber(int[] nums) {
        if (nums == null || nums.length == 0)
            return -1;
        int res = 0;
        for (int i = 0; i < 32; i++) {
            int cnt = 0;
            int bit = 1 << i;
            for (int n : nums) {
                // 这里的判断条件为不等于0，说明该位上如果是0就不加，如果是1就加上；
                // 但是不能将其改为等于1，因为这个计算结果不是仅有这一位的与结果，除0之外还有很多可能的值；
                // 因此要用不等于0进行判断。
                if ((n & bit) != 0)
                    cnt++;
            }
            if (cnt % 3 != 0)
                res |= bit;
        }
        return res;
    }
}
```
* 这种做法还可以扩展到“一个数字出现1次，其余数字出现k次”的问题上，只需把`cnt % 3 != 0`换为`cnt % k != 0`即可。
2. 法二（位运算）