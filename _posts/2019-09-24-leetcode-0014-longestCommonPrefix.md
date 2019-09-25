---
layout: post
title: LeetCode 0014 题解
description: "最长公共前缀"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

### 思路
纵向比较每一个字符串的相同位置上的字符，需两重循环，外层为位置的变化，内层为字符串的顺序变化。当某一字符串结束时，或没有相同字符时结束。

### 题解

```java
class Solution {
    public static String longestCommonPrefix(String[] strs) {
        // if判断中顺序不能换，一定要先判断数组长度，否则不存在strs[0]，报错。
        if (strs.length == 0 || strs[0] == null) return ""; 
        for (int i = 0; i < strs[0].length(); i++){
            for (int j = 1; j < strs.length; j++){
                // 同理，先进行判断第i个位置是否存在，再判断是否相等。
                if (i == strs[j].length() || strs[0].charAt(i) != strs[j].charAt(i))
                return strs[0].substring(0, i);
            }
        }
        return strs[0];
    }
}
```
### 思考
原来在第一层循环时，无法确定$i$的结束位置，于是想找到数组中最短字符串的长度，但这样意味着要将所有字符串扫描一遍，显然不好。  
看完解答后发现可以像上面一样，在第二个`if`中进行判断，学习！

