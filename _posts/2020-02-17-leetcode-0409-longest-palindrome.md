---
layout: post
title: LeetCode 0409 题解
description: "最长回文串"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)

### 题解
1. 法一（哈希表）：哈希表存储各字符及其出现次数，再扫描哈希表，若出现次数为偶数，则加入长度中，若为奇数，则将出现次数减1加入长度中，这个操作可以用简单的一行代码实现。最后，如果长度比字符串长度短，那说明可以再取一个次数为奇数的字符，将它放在回文串中间，因此需要加1。
```java
class Solution {
    public int longestPalindrome(String s) {
        if(s == null || s.length() == 0)
            return 0;
        Map<Character, Integer> map = new HashMap<>();
        int cnt = 0;
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        for(char c : map.keySet())
            cnt += map.get(c) / 2 * 2;
        if(cnt < s.length())
            cnt++;
        return cnt;
    }
}
```
2. 法二（数组）：由于字符数有限，可以用固定长度的数组存储各字符的出现情况。
```java
class Solution {
    public int longestPalindrome(String s) {
        if(s == null || s.length() == 0)
            return 0;
        int[] count = new int[256];
        int cnt = 0;
        for(char c : s.toCharArray())
            count[c]++;
        for(int i : count)
            cnt += i / 2 * 2;
        if(cnt < s.length())
            cnt++;
        return cnt;
    }
}
```
* 如果字符串中包含的字符种类太多，那么哈希表要比数组合适。