---
layout: post
title: LeetCode 0042 题解
description: "接雨水"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

### 题解
1. 法一（暴力法）：对于每一个元素，分别找到在它左边和右边的最大值，则这两个最大值的较小值减去当前元素即为当前位置能接到的雨水。
```java
class Solution {
    public int trap(int[] height) {
        int water = 0;
        for(int i = 0; i < height.length; i++){
            int maxLeft = 0, maxRight = 0;
            for(int j = i; j >= 0; j--)
                maxLeft = Math.max(maxLeft, height[j]);
            for(int j = i; j < height.length; j++)
                maxRight = Math.max(maxRight, height[j]);
            water += Math.min(maxLeft, maxRight) - height[i];
        }
        return water;
    }
}
```
2. 法二（动态规划）：法一中对于每个元素都要遍历两边找到最大值，可以将两边的最大值分别存储在两个数组中，最后遍历数组进行雨水的计算。
```java
class Solution {
    public int trap(int[] height) {
        if(height == null || height.length == 0)
            return 0;
        int water = 0;
        int[] maxLeft = new int [height.length];
        maxLeft[0] = height[0];
        for(int i = 1; i < height.length; i++)
            maxLeft[i] = Math.max(maxLeft[i-1], height[i]);
        int[] maxRight = new int [height.length];
        maxRight[height.length-1] = height[height.length-1];
        for(int i = height.length - 2; i >= 0; i--)
            maxRight[i] = Math.max(maxRight[i+1], height[i]);
        for(int i = 0; i < height.length; i++)
            water += Math.min(maxLeft[i], maxRight[i]) - height[i];
        return water;
    }
}
```
3. 法三（双指针法）：由动态规划的方法可知，每一个元素能接到的雨水等于其左侧、右侧中最大值的较小值减去当前元素，因此采用双指针。当左侧最大值较小时，移动左侧指针`i`，并将当前元素的雨水加入总数，同时更新左侧最大值；同理，当右侧最大值较小时，移动右指针`j`，加入当前雨水并更新右侧最大值。
```java
class Solution {
    public int trap(int[] height) {
        if(height == null || height.length == 0)
            return 0;
        int i = 0, j = height.length - 1;
        int maxLeft = height[i], maxRight = height[j], water = 0;
        while(i < j){
            if(maxLeft <= maxRight){
                water += maxLeft - height[i];
                i++;
                maxLeft = Math.max(maxLeft, height[i]);
            }
            else{
                water += maxRight - height[j];
                j--;
                maxRight = Math.max(maxRight, height[j]);
            }
        }
        return water;
    }
}
```
4. 法四（栈）：若当前元素大于栈顶，则先将栈顶元素出栈并记录为`top`，随后计算现在栈顶与当前元素之间的雨水：【它们之间的距离】乘上【它们中的较小值减去`top`的差值】。
```java
class Solution {
    public int trap(int[] height) {
        if(height == null || height.length == 0)
            return 0;
        int current = 0, water = 0;
        Stack<Integer> stack = new Stack<>();
        while(current < height.length){
            while(!stack.isEmpty() && height[current] > height[stack.peek()]){
                int top = stack.pop();
                if(stack.isEmpty())
                    break;
                int distance = current - stack.peek() - 1;
                int min = Math.min(height[current], height[stack.peek()]);
                water += (min - height[top]) * distance;
            }
            stack.push(current++);
        }
        return water;
    }
}
```