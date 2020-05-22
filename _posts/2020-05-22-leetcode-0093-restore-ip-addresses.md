---
layout: post
title: LeetCode 0093 题解
description: "复原IP地址"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> restoreIpAddresses(String s) {
        if (s == null || s.length() < 4 || s.length() > 12)
            return res;
        backtrace(0, s, new StringBuilder());
        return res;
    }
    // k用于记录回溯的次数，相当于已经分割出了多少个字符串，到4就应该返回。
    private void backtrace(int k, String s, StringBuilder path) {
        if (k == 4 || s.length() == 0) {
            if (k == 4 && s.length() == 0)
                res.add(path.toString());
            return;
        }
        for (int i = 0; i < s.length() && i <= 2; i++) {
            // 只能出现单个0，不能出现0后面还有数字的情况
            if (i != 0 && s.charAt(0) == '0')
                break;
            String part = s.substring(0, i + 1);
            if (Integer.valueOf(part) <= 255) {
                // 除第一个截取的字符串外，其余都要在前面加上.号。
                if (path.length() != 0) {
                    part = "." + part;
                }
                path.append(part);
                // 回溯时前i个字符已经考虑完了，因此传入从i+1开始的字符串。
                backtrace(k + 1, s.substring(i + 1), path);
                path.delete(path.length() - part.length(), path.length());
            }
        }
    }
}
```