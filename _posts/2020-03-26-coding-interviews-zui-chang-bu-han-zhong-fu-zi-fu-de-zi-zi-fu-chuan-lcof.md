---
layout: post
title: 剑指Offer 48 题解
description: "最长不含重复字符的子字符串"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

### 题解
1. 法一（滑动窗口）：窗口内是不含重复字符的字符串，如果当前字符在窗口内出现了，就将左边界`left`右移，直至不含该字符；反之，则将右边界`right`右移。如果不使用`set`，则在判断当前字符是否在窗口出现时，需要逐个扫描，时间复杂度为 $O(n^2)$ ；使用`set`后，时间复杂度降为 $O(n)$ ，空间复杂度升为 $O(n)$ 。
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0)
            return 0;
        Set<Character> set = new HashSet<>();
        int res = 0;
        for (int left = 0, right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            while (set.contains(c)) {
                set.remove(s.charAt(left++));
            }
            set.add(c);
            res = Math.max(res, right - left + 1);
        }
        return res;
    }
}
```