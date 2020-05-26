---
layout: post
title: LeetCode 0131 题解
description: "分割回文串"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<List<String>> res = new ArrayList<>();
    public List<List<String>> partition(String s) {
        if (s == null || s.length() == 0)
            return res;
        backtrace(s, new ArrayList<String>());
        return res;
    }
    private void backtrace(String s, ArrayList<String> path) {
        if (s.length() == 0) {
            res.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < s.length(); i++) {
            if (isPalindrome(s, 0, i)) {
                path.add(s.substring(0, i + 1));
                backtrace(s.substring(i + 1), path);
                path.remove(path.size() - 1);
            }
        }
    }
    private boolean isPalindrome(String s, int start, int end) {
        while (start < end) {
            if (s.charAt(start) != s.charAt(end))
                return false;
            start++;
            end--;
        }
        return true;
    }
}
```