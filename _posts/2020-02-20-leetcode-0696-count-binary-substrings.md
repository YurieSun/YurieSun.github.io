---
layout: post
title: LeetCode 0696 题解
description: "计数二进制子串"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[计数二进制子串](https://leetcode-cn.com/problems/count-binary-substrings/)

### 题解
1. 法一：对于相邻两组0和1，可以构成满足条件的子串数量为0和1的个数的较小值。因此，可以用一个数组存储字符串中每一段连续的0和1的个数，并再次对数组扫描，来统计总个数。
```java
class Solution {
    public int countBinarySubstrings(String s) {
        if(s == null || s.length() == 0)
            return 0;
        int t = 0;
        int[] group = new int[s.length()];
        group[0] = 1;
        for(int i = 1; i < s.length(); i++){
            if(s.charAt(i) == s.charAt(i - 1))
                group[t]++;
            else
                group[++t] = 1;
        }
        int cnt = 0;
        for(int i = 1; i < group.length; i++)
            cnt += Math.min(group[i - 1], group[i]);
        return cnt;
    }
}
```
* 改进：每次只用到了相邻两个值，因此可以用两个变量来代替数组进行存储。特别要注意到达字符末尾时，没有进入else的条件中，使得最后两组的值没有统计，因此还需加上最后一组的值。
```java
class Solution {
    public int countBinarySubstrings(String s) {
        if(s == null || s.length() == 0)
            return 0;
        int pre = 0, cur = 1;
        int cnt = 0;
        for(int i = 1; i < s.length(); i++){
            if(s.charAt(i) == s.charAt(i - 1))
                cur++;
            else{
                cnt += Math.min(pre, cur);
                pre = cur;
                cur = 1;
            }
        }
        return cnt + Math.min(pre, cur);
    }
}
```
2. 法二：对于一个与之前数字不一样的序列，只要前面序列的个数大于等于当前序列，就可以出现一个满足要求的子串。因此，在碰到新序列时，可以在对序列计数的同时更新可构造的子串总数。
```java
class Solution {
    public int countBinarySubstrings(String s) {
        if(s == null || s.length() == 0)
            return 0;
        int pre = 0, cur = 1;
        int cnt = 0;
        for(int i = 1; i < s.length(); i++){
            if(s.charAt(i) == s.charAt(i - 1))
                cur++;
            else{
                pre = cur;
                cur = 1;
            }
            if(pre >= cur)
                cnt++;
        }
        return cnt;
    }
}
```