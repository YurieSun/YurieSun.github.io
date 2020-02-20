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
    private String res;
    private int max;
    public String longestPalindrome(String s) {
        if(s == null || s.length() == 0)
            return s;
        for(int i = 0; i < s.length(); i++){
            extendStr(s, i, i);
            extendStr(s, i, i + 1);
        }
        return res;
    }
    private void extendStr(String s, int start, int end){
        while(start >= 0 && end < s.length() && s.charAt(start) == s.charAt(end)){
            start--;
            end++;
        }
        //循环结束后start所在的位置相对于真正的子字符串开始位置左移了1，同理end所在位置在结束位置右边1位；
        //因此子字符串长度为end-start+1-2 = end-start-1。
        int len = end - start - 1;
        if(max < len){
            max = len;
            res = s.substring(start + 1, end);
        }        
    }
}
```
* 改进：上述方法在每次找到较大长度的回文串时，都要使用一次`substring()`，会增加时间复杂度；且字符串有奇数个字符和有偶数个字符的情况都算了相应的最大子串。因此，通过先比较奇数和偶数两种情况下的大小，同时通过更新最长回文串的起始位置与长度，只在返回时使用一次`substring()`，就可以缩短时间。但LeetCode测试下，这种方法所花时间比上述方法更长。
```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.length() == 0)
            return s;
        int len = 0, start = 0;
        for(int i = 0; i < s.length(); i++){
            int cur = Math.max(extendStr(s, i, i), extendStr(s, i, i + 1));
            if(cur > len){
                len = cur;
                start = i - (len - 1) / 2;
            }
        }
        return s.substring(start, start + len);
    }
    private int extendStr(String s, int l, int r){
        while(l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)){
            l--;
            r++;
        }
        return r - l - 1;      
    }
}
```
2. 法二（动态规划）：数组中`dp[i][j]`表示起点为`i`终点为`j`的字符串是否为回文，而根据`dp[i+1][j-1]`是否为回文且第`i`个字符与第`j`个字符是否相等，就可以判断出`dp[i][j]`的值。本质上和法一的扩展字符串一样，都是从中心开始判断扩展后的字符串。
```java
class Solution {
    public String longestPalindrome(String s) {
        if(s == null || s.length() == 0)
            return s;
        int len = s.length();
        int start = 0, max = 0;
        boolean[][] dp = new boolean[len][len];
        for(int i = len - 1; i >= 0; i--)
            for(int j = 0; j < len; j++){
                if(s.charAt(i) == s.charAt(j) && (j - i < 2 || dp[i+1][j-1])){
                    dp[i][j] = true;
                    if(j - i + 1 > max){
                        max = j - i + 1;
                        start = i;
                    }
                }
            }
        return s.substring(start, start + max);   
    }
}
```
* 和第647题 [回文子串](https://leetcode-cn.com/problems/palindromic-substrings/) 的做法是相同的。