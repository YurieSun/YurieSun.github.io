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
            for(int j = i + 1; j < T.length; j++){
                if(T[j] > T[i]){
                    res[i] = j - i;
                    break;
                }
            }
        }
        return res;  
    }
}
```
* 改进：上述方法中，会出现重复访问的情况。因此，可以从后往前遍历数组，并比较当前元素`cur`与其后一个元素`next`的大小；若`cur`大，则通过`next`找到比`next`大的元素，并进行比较；若`cur`小，则`next`就是最开始比它大的元素。通过后面元素的信息，可以减少元素比较次数。
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        if(T == null || T.length == 0)
            return res;
        for(int i = T.length - 2; i >= 0; i--){
            //j下标的递增就是利用后面元素的信息
            for(int j = i + 1; j < T.length; j += res[j])
                if(T[j] > T[i]){
                    res[i] = j - i;
                    break;
                }
                //这是T[j] <= T[i]的情况，因此如果没有比T[j]大的数，肯定也没有比T[i]大的数。
                else if(res[j] == 0){
                    res[i] = 0;
                    break;
                }
        }        
        return res;  
    }
}
```
2. 法二（栈）：建立一个递减栈，栈中存放的是元素下标。若当前元素比栈顶下标对应的元素大，则说明该数是栈顶下标对应的元素遇到的第一个比它大的元素，则将下标差值存入数组对应的位置即可；若当前元素比栈顶下标对应的元素小，则将下标入栈。
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