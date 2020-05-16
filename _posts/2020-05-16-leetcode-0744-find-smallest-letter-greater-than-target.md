---
layout: post
title: LeetCode 0744 题解
description: "寻找比目标字母大的最小字母"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

### 题解
1. 法一（二分）
```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int left = 0, right = letters.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (letters[mid] == target)
                left = mid + 1;
            else if (letters[mid] > target)
                right = mid - 1;
            else if (letters[mid] < target)
                left = mid + 1;
        }
        if (left == letters.length)
            return letters[0];
        return letters[left];
    }
}
```