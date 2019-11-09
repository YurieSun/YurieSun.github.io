---
layout: post
title: LeetCode 0118 题解
description: "杨辉三角"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)

### 思路
根据计算规则进行计算，主要学习的是`List`集合的使用方法。

### 题解
```java
class Solution {
    public List<List<Integer>> generate(int numRows) { 
        List<List<Integer>> ans = new ArrayList<>();
        if(numRows == 0) return ans;
        ans.add(new ArrayList<Integer>());
        ans.get(0).add(1);
        for(int i = 1; i < numRows; i++){
            List<Integer> row = new ArrayList<>();
            row.add(1);
            for(int j = 1; j < i; j++)
                row.add(ans.get(i-1).get(j-1) + ans.get(i-1).get(j));
            row.add(1);
            ans.add(row);
        }
        return ans;
    }
}
```