---
layout: post
title: LeetCode 0739 题解
description: "每日温度"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

### 题解
1. 法一（二重循环）：找到每个元素之后的较大值的位置。
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        if(T == null || T.length == 0)
            return res;
        for(int i = 0; i < T.length; i++){
            int cnt = 0;
            for(int j = i + 1; j < T.length; j++){
                cnt++;
                if(T[j] > T[i]){
                    res[i] = cnt;
                    break;
                }
            }
        }
        return res;  
    }
}
```
2. 法二（栈）：建立一个递减栈，栈中存放的是元素下标。若当前元素比栈顶下标表示的元素大，则说明该数是栈顶栈顶下标表示的元素遇到的第一个比它大的元素，则将下标差值存入数组对应的位置即可；若当前元素比栈顶下标表示的元素小，则将下标入栈。
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        if(T == null || T.length == 0)
            return res;
        Stack<Integer> stack = new Stack<>();
        for(int i = 0; i < T.length; i++){
            while(!stack.isEmpty() && T[i] > T[stack.peek()]){
                int pre = stack.pop();
                res[pre] = i - pre;

            }
            stack.push(i);
        }        
        return res;  
    }
}
```