---
layout: post
title: LeetCode 0830 题解
description: "较大分组的位置"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[较大分组的位置](https://leetcode-cn.com/problems/positions-of-large-groups/)

### 题解
1. 法一：遍历字符串，出现相同字母时进入`while`循环统计次数，若次数大于等于3，则将下标数组存入集合。
```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String S) {
        List<List<Integer>> list = new ArrayList<>();
        int count = 1;
        for(int i = 1; i < S.length(); i++){
            if(S.charAt(i-1) == S.charAt(i)) {
            	while(i < S.length() && S.charAt(i-1) == S.charAt(i)) {
            		i++;
            		count++;
            	}
            }
        	if(count >= 3)
        		list.add(Arrays.asList(new Integer[]{i-count, i-1}));
        	count = 1;
        }
        return list;
    }
}
```
2. 法二：遍历字符串，出现相同字母时更新次数`count`，一旦不相等则判断次数是否大于等于3，同时将`count`更新为1。（这个方法在字符串后面加上`0`，是为了判断整个字符串均为同一字母的情况，否则这种情况无法执行将数组下标存入集合的操作）
```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String S) {
        List<List<Integer>> list = new ArrayList<>();
        int count = 1;
        S += '0';
        for(int i = 1; i < S.length(); i++){
            if(S.charAt(i-1) == S.charAt(i)) 
                count++;
            else{
            	if(count >= 3)
            		list.add(Arrays.asList(new Integer[]{i-count, i-1}));
        	    count = 1;
            }
        }
        return list;
    }
}
```
3. 法三：遍历字符串，记录相同字符的起始下标与结束下标。这里要注意的是循环变量`i`的结束值被设为`S.length()`，同样是为了判断判断整个字符串均为同一字母的情况。
```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String S) {
        List<List<Integer>> list = new ArrayList<>();
        int start = 0, end = 0;
        for(int i = 1; i <= S.length(); i++){
            if(i == S.length() || S.charAt(i-1) != S.charAt(i)) {
            // i == S.length()这个判断条件应放在前面，防止下标溢出
                if(end - start >= 2)
                    list.add(Arrays.asList(new Integer[]{start, end}));
                start = i;   
            }
            end = i;
        }
        return list;
    }
}
```