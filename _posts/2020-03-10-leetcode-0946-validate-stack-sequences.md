---
layout: post
title: LeetCode 0946 题解
description: "验证栈序列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)

### 题解
1. 法一：遍历`pushed`数组，将数组元素放入栈中，并同时检查，若栈顶元素与`popped`数组当前元素相等，则出栈。最后检查`popped`数组中的元素是否全部出栈。
```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int i = 0;
        for (int n : pushed) {
            stack.push(n);
            while (!stack.isEmpty() && i < popped.length && stack.peek() == popped[i]) {
                stack.pop();
                i++;
            }
        }
        return i == pushed.length;
    }
}
```