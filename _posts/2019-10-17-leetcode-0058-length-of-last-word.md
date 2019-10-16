---
layout: post
title: LeetCode 0058 题解
description: "最后一个单词的长度"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最后一个单词的长度](https://leetcode-cn.com/problems/length-of-last-word/)

### 思路
首先删去尾部空白符，然后从后往前扫描，遇到空格结束返回。

### 题解

```java
class Solution {
    public int lengthOfLastWord(String s) {
        String str = s.trim();
        int count = 0;
        for(int i = str.length()-1; i >= 0; i--){
            if(str.charAt(i) == ' ') break;
            count++;
        }
        return count;
    }
}
```