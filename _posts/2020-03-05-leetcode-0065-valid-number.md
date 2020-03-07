---
layout: post
title: LeetCode 0065 题解
description: "有效数字"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[有效数字](https://leetcode-cn.com/problems/valid-number/)

### 题解
1. 法一：将字符串通过`e`分割成底数和指数，并分别判断。底数中只能含正负号、数字和小数点，指数中只能含正负号和数字。
```java
class Solution {
    public boolean isNumber(String s) {
        if(s == null || s.length() == 0)
            return false;
        // 去掉首尾空格
        s = s.trim();
        String base = s, exp = null;
        // 分割底数和指数
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == 'e'){
                base = s.substring(0, i);
                exp = s.substring(i+1, s.length());
            }
        }
        if(exp == null)
            return isValidBase(base);
        return isValidBase(base) && isValidExp(exp);
    }
    // 判断底数是否有效
    private boolean isValidBase(String s){
        boolean res = false;
        boolean pointed = false;
        for(int i = 0; i < s.length(); i++){
            if(s.charAt(i) == '-' || s.charAt(i) == '+') {
            	if(i != 0)
            		return false;
            }
            else if(s.charAt(i) == '.'){
                if(pointed)
                    return false;
                pointed = true;
            }
            else if(s.charAt(i) < '0' || s.charAt(i) > '9')
                return false;
            else
            	res = true;
        }
        return res;
    }
    // 判断指数是否有效
    private boolean isValidExp(String s){
        boolean res = false;
        for(int i = 0; i < s.length(); i++){
        	if(s.charAt(i) == '-' || s.charAt(i) == '+') {
            	if(i != 0)
            		return false;
            }
            else if(s.charAt(i) < '0' || s.charAt(i) > '9')
                return false;
            
            else
            	res = true;
        }
        return res;
    }
}
```
2. 法二（状态机）
```java
class Solution {
    public int make(char c) {
        switch(c) {
            case ' ': return 0;
            case '+':
            case '-': return 1;
            case '.': return 3;
            case 'e': return 4;
            default:
                if(c >= 48 && c <= 57) return 2;
        }
        return -1;
    }
    
    public boolean isNumber(String s) {
        int state = 0;
        int finals = 0b101101000;
        // 二维数组的定义总是会导致页面出现错误，具体定义见下图。
        int[][] transfer;
        char[] ss = s.toCharArray();
        for(int i=0; i < ss.length; ++i) {
            int id = make(ss[i]);
            if (id < 0) return false;
            state = transfer[state][id];
            if (state < 0) return false;
        }
        return (finals & (1 << state)) > 0;
    }
}
```
问题：如何构建有限状态机（DFA）？本题中DFA对应的二维数组表示如下图所示：  
![DFA对应的二维数组]({{ '/assets/images/LeetCode/0065.PNG' }})

3. 法三（正则表达式）：通过正则表达式进行匹配。
```java
/*
    [] ： 字符集合
    () ： 分组
    ? ： 重复 0 ~ 1 次
    + ： 重复 1 ~ n 次
    * ： 重复 0 ~ n 次
    . ： 任意字符
    \\. ： 转义后的 .
    \\d ： 数字
*/
class Solution {
    public boolean isNumber(String s) {
        return s.trim().matches("[-+]?(\\d+\\.?|\\.\\d+)\\d*(e[-+]?\\d+)?");
    }
}
```