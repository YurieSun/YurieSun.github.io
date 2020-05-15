---
layout: post
title: LeetCode 0763 题解
description: "划分字母区间"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[划分字母区间](https://leetcode-cn.com/problems/partition-labels/)

### 题解
1. 法一（贪心）：先遍历字符串，记录每个字母最后出现的位置。再次遍历字符串，其中`start`和`end`分别表示当前片段的开头和结尾，在遍历过程中，不断扩展`end`的位置，直到当前位置到达了结尾，则表明从`start`到`end`之间的字符串可以包含已经出现的字符。
```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> res = new ArrayList<>();
        if (S == null || S.length() == 0)
            return res;
        int[] pos = new int[26];
        for (int i = 0; i < S.length(); i++)
            pos[S.charAt(i) - 'a'] = i;
        int start = 0, end = 0;
        for (int i = 0; i < S.length(); i++) {
            int index = pos[S.charAt(i) - 'a'];
            if (index > end) {
                end = index;
            }
            if (i == end) {
                res.add(end - start + 1);
                start = end + 1;
            }
        }
        return res;
    }
}
```