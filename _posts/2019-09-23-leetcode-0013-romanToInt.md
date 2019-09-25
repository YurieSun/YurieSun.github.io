---
layout: post
title: LeetCode 0013 题解
description: "罗马数字转整数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

### 思路
将字符与数字的对应关系放入hashtable，将字符串的每个字符从右至左依次读取，若前一个比现在的大或相等，则相加；若前一个比现在的小，则相减。

### 题解

```java
class Solution {
    public static int romanToInt(String s) {
        Map<Character, Integer> map = new HashMap<>(); 
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);
        int ans = map.get(s.charAt(s.length()-1));
        if(s.length()-1 == 0) {}
        else{
            for (int i = s.length()-1; i > 0; i--){
                char current = s.charAt(i);
                char pre = s.charAt(i-1);
                if(map.get(current) <= map.get(pre))
                    ans += map.get(pre);
                else
                    ans -= map.get(pre);
            }
        }
        return ans;
    }
}
```
### 思考
其中提取字符串的单个字符可采用两种方法：  
 String 的`substring(i, j)`可获得$i$到$j-1$的子字符串，返回值仍为 String;  
 String 的`charAt(i)`可获得第$i$个字符，返回值为char。

