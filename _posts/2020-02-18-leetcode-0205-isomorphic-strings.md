---
layout: post
title: LeetCode 0205 题解
description: "同构字符串"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

### 题解
1. 法一（哈希表）：哈希表存储两个字符串中对应的字符对，再进行判断。但这里有个问题，比如输入`ab`和`aa`，也会返回`true`，因为第二对会将原来的覆盖掉。因此，将其作为一个函数，并进行双向判断即可。
```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        return helper(s, t) && helper(t, s);
    }
    private boolean helper(String s, String t){
        Map<Character, Character> map = new HashMap<>();
        for(int i = 0; i < s.length(); i++){
            char c1 = s.charAt(i);
            char c2 = t.charAt(i);
            if(map.containsKey(c1)){
                if(c2 != map.get(c1))
                    return false;
            }   
            else
                map.put(c1, c2);
        }
        return true;
    }
}
```
2. 法二（数组）：用数组记录字符对的出现情况，数组元素为`0`说明该字符未出现，如果出现了则标记为`i+1`（为了避免和`0`冲突，不能标记为`i`），则在后面碰上字符对时，只需判断相应的标记是否相同即可。
```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        int[] sArr = new int[256];
        int[] tArr = new int[256];
        for(int i = 0; i < s.length(); i++){
            char sc = s.charAt(i);
            char tc = t.charAt(i);
            if(sArr[sc] != tArr[tc])
                return false;
            sArr[sc] = i + 1;
            tArr[tc] = i + 1;
        }
        return true;
    }
}
```