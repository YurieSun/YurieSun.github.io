---
layout: post
title: LeetCode 0647 题解
description: "回文子串"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

### 题解
1. 法一：从第一个字符开始，向左右两边扩展字符串，并判断是否为回文子串。
```java
class Solution {
    private int cnt = 0;
    public int countSubstrings(String s) {
        if(s == null || s.length() == 0)
            return 0;
        for(int i = 0; i < s.length(); i++){
            extendStr(s, i, i); //字符串的字符个数为奇数
            extendStr(s, i, i + 1); //字符串的个数为偶数
        }
        return cnt;
    }
    private void extendStr(String s, int start, int end){
        while(start >= 0 && end < s.length() && s.charAt(start) == s.charAt(end)){
            start--;
            end++;
            cnt++;
        }
    }
}
```
2. 法二（动态规划）：数组中`dp[i][j]`表示起点为`i`终点为`j`的字符串是否为回文，而根据`dp[i+1][j-1]`是否为回文且第`i`个字符与第`j`个字符是否相等，就可以判断出`dp[i][j]`的值。本质上和法一的扩展字符串一样，都是从中心开始判断扩展后的字符串。
```java
class Solution {
    private int cnt = 0;
    public int countSubstrings(String s) {
        if(s == null || s.length() == 0)
            return 0;
        int len = s.length(), cnt = 0;
        boolean[][] dp = new boolean[len][len];
        for(int i = len - 1; i >= 0; i--)
            for(int j = i; j < len; j++){
                //j - i < 2的情况可以看成字符数分别为奇数和偶数的初始化，因为字符数为1、2的字符串且字符相等一定是回文串；
                //相当于法一中按照字符个数为奇数、偶数执行了两次，这里也需要两个初始化。
                if(s.charAt(i) == s.charAt(j) && (j - i < 2 || dp[i+1][j-1])){
                    dp[i][j] = true;
                    cnt++;
                }
            }
        return cnt;
    }
}
```