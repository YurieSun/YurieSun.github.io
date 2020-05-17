---
layout: post
title: LeetCode 0241 题解
description: "为运算表达式设计优先级"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[为运算表达式设计优先级](https://leetcode-cn.com/problems/different-ways-to-add-parentheses/)

### 题解
1. 法一（分治）：以运算符为分界，将字符串分成左、右两部分，并计算结果。
```java
class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> res = new ArrayList<>();
        if (input == null || input.length() == 0)
            return res;
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (c == '+' || c == '-' || c == '*') {
                // 将字符串根据运算符分为两部分
                List<Integer> left = diffWaysToCompute(input.substring(0, i));
                List<Integer> right = diffWaysToCompute(input.substring(i + 1));
                for (int l : left) {
                    for (int r : right) {
                        switch (c) {
                            case '+':
                                res.add(l + r);
                                break;
                            case '-':
                                res.add(l - r);
                                break;
                            case '*':
                                res.add(l * r);
                                break;
                        }
                    }
                }
            }
        }
        // 当input中没有运算符时，说明这是一个数字，将这个数字放入结果集即可。
        if (res.size() == 0)
            res.add(Integer.valueOf(input));
        return res;
    }
}
```