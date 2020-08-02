---
layout: post
title: LeetCode 0318 题解
description: "最大单词长度乘积"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最大单词长度乘积](https://leetcode-cn.com/problems/maximum-product-of-word-lengths/)

### 题解
1. 法一：本题的主要问题在于如何判断两字符串是否存在相同字符，一种方法是两重循环，对两个字符串中的每个字符都进行比较。另一种方法是，本题只涉及小写字母，共26个，因此每个字符串出现的字符可以用一个32位`int`表示；预处理完之后，若两个字符串对应的`int`相与等于0，就说明没有相同字符。
```java
class Solution {
    public int maxProduct(String[] words) {
        int[] cnt = new int[words.length];
        // 预处理，将每个字符串中出现的字符位置在对应int的位置上置1。
        for (int i = 0; i < words.length; i++) {
            for (char c : words[i].toCharArray()) {
                cnt[i] |= (1 << c - 'a');
            }
        }
        int res = 0;
        for (int i = 0; i < words.length; i++) {
            for (int j = i + 1; j < words.length; j++) {
                // 相与为0说明两个字符串不存在相同字符，则更新最大长度。
                if ((cnt[i] & cnt[j]) == 0) {
                    res = Math.max(res, words[i].length() * words[j].length());
                }
            }
        }
        return res;
    }
}
```