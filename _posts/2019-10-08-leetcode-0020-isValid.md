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
* 改进  
1. 用hashmap的方式存放会更好，一定要注意哪个元素作为key，哪个作为value；
2. 在遇到右括号出栈时，若为空则赋一个其他值；
3. 最后处理完所有字符后可直接返回isEmpty()，省略if判断。

```java
class Solution {
    public static boolean isValid(String s) {
        Stack<Character> str = new Stack<>();
        Map<Character, Character> map = new HashMap<>();
        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');
        for (int i = 0; i < s.length(); i++){
            char c = s.charAt(i);
            if(map.containsKey(c)){
                char top = str.isEmpty() ? '#' : str.pop();
                if(map.get(c) != top) return false;
            } 
            else
                str.push(c);
        }
        return str.isEmpty();
    }
}
```
### 思考
其中若字符为右括号时，首先要查看栈是否为空再取元素，否则会报错。

