---
layout: post
title: LeetCode 0020 题解
description: "有效的括号"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

### 思路
依次读取字符串中每个字符，若为空格，忽略；若为左括号，入栈；若为右括号时，有两种情况，当栈为空，则返回false，否则从栈中取出一个字符，判断是否为其对应的左括号。当字符串处理完成，栈非空，也返回false。

### 题解

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> str = new Stack<>();
        for (int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(c == ' ') continue;
            if(c == '(' || c == '[' || c == '{')
                str.push(c);
            else{
                if (str.isEmpty()) return false;
                char out = str.pop();
                if(c == ')' && out != '(') return false;
                if(c == ']' && out != '[') return false;
                if(c == '}' && out != '{') return false;
            } 
        }
        if(!str.isEmpty()) return false;
        return true;
    }
}
```
### 思考
其中若字符为右括号时，首先要查看栈是否为空再取元素，否则会报错。

