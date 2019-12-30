---
layout: post
title: LeetCode 0011 题解
description: "盛最多水的容器"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

### 题解
1. 法一（暴力法）：二重循环计算所有面积，并更新最大值。
```java
class Solution {
    public int maxArea(int[] height) {
        int max = 0;
        for(int i = 0; i < height.length; i++)
            for(int j = i + 1; j < height.length; j++){
                int area = (j - i) * Math.min(height[i], height[j]);
                if(area > max)
                    max = area;
            }
        return max;
    }
}
```
2. 法二（双指针法）：两个指针分别指向数组开头与末尾，比较两元素大小，指向较小元素的指针向中间移动，并更新面积最大值。（面积大小由数组中较小元素和两元素之间的距离一起决定，若指针从两端往中间走，则两元素之间的距离越来越小。如果每次移动较大元素的指针，其面积只会越来越小，因为两元素最小值不变或变小，这样是没有意义的；如果每次移动较小元素的指针，则在尝试能否找到比当前的较小值更大的元素，且每次移动排除了肯定比当前面积小的情况。这种方法的具体证明可以参考LeetCode上的题解。）
```java
class Solution {
    public int maxArea(int[] height) {
        int max = 0, area = 0;
        int i = 0, j = height.length - 1;
        while(i < j){
            if(height[i] > height[j]){
                area = (j - i) * height[j];
                j--;
            }      
            else{
                area = (j - i) * height[i];
                i++;
            }
            max = Math.max(max, area);
        }
        return max;
    } 
}
```
