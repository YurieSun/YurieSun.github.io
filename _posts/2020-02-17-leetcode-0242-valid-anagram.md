---
layout: post
title: LeetCode 0242 题解
description: "有效的字母异位词"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

### 题解
1. 法一（排序）：对两个字符串进行排序，检查是否相等。
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length())
            return false;
        char[] sArr = s.toCharArray();
        char[] tArr = t.toCharArray();
        Arrays.sort(sArr);
        Arrays.sort(tArr);
        return Arrays.equals(sArr, tArr);
    }
}
```
2. 法二（哈希表）：扫描字符串`s`，并将每个字符及其出现次数存储在哈希表上；再扫描字符串`t`，检查每个字符出现的次数是否与哈希表的相同。
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length())
            return false;
        Map<Character, Integer> map = new HashMap<>();
        for(int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        for(int i = 0; i < t.length(); i++){
            char c = t.charAt(i);
            if(!map.containsKey(c))
                return false;
            map.put(c, map.get(c) - 1);
            if(map.get(c) < 0)
                return false;
        }
        return true;
    }
}
```
3. 法三（数组）：由于字符数有限，可以用固定长度的数组存储各字符的出现情况。
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length())
            return false;
        int[] count = new int[26];
        for(int i = 0; i < s.length(); i++){
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }
        for(int i : count)
            if(i != 0)
                return false;
        return true;
    }
}
```
* 如果字符串中包含的字符种类太多，那么哈希表要比数组合适。