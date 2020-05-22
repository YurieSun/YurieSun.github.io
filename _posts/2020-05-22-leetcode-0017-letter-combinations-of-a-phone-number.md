---
layout: post
title: LeetCode 0017 题解
description: "电话号码的字母组合"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

### 题解
1. 法一（回溯）
```java
class Solution {
    private List<String> res = new ArrayList<>();
    // map中存放每个数字对应的字符串，前两个置空是为了让数字和下标一一对应。
    String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0)
            return res;
        backtrace(digits, 0, new StringBuilder());
        return res;
    }
    // 通过index指针记录当前字符串正在被解析的数字位置
    private void backtrace(String digits, int index, StringBuilder path) {
        if (index == digits.length()) {
            res.add(path.toString());
            return;
        }
        // 先找到当前数字，并从map中找到对应的字符
        char c = digits.charAt(index);
        String cur = map[c - '0'];
        for (char ch : cur.toCharArray()) {
            path.append(ch);
            backtrace(digits, index + 1, path);
            path.deleteCharAt(path.length() - 1);
        }
    }
}
```