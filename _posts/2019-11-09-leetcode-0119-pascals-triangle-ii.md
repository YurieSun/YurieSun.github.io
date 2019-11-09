---
layout: post
title: LeetCode 0119 题解
description: "杨辉三角 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[杨辉三角 II](https://leetcode-cn.com/problems/pascals-triangle-ii/)

### 思路
根据第118题的思路，构造杨辉三角的集合，并返回最后一行。

### 题解
```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<List<Integer>> triangle = new ArrayList<>();
        triangle.add(new ArrayList<Integer>());
        triangle.get(0).add(1);
        for(int i = 1; i < rowIndex+1; i++){
            List<Integer> row = new ArrayList<>();
            row.add(1);
            for(int j = 1; j < i; j++)
                row.add(triangle.get(i-1).get(j-1) + triangle.get(i-1).get(j)); 
            row.add(1);
            triangle.add(row);
        }
        return triangle.get(rowIndex);
    }
}
```
* 改进一：不保存每一行，只保存前一行，用来计算当前行。
```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> preRow = new ArrayList<>();
        preRow.add(1);
        for(int i = 1; i < rowIndex+1; i++){
            List<Integer> row = new ArrayList<>();
            row.add(1);
            for(int j = 1; j < i; j++)
                row.add(preRow.get(j-1) + preRow.get(j)); 
            row.add(1);
            preRow = row;
        }
        return preRow;
    }
}
```
两重`for`循环也可写成：
```java
for(int i = 1; i < rowIndex+1; i++){
    List<Integer> row = new ArrayList<>();
    for(int j = 0; j <= i; j++)
        if(j == 0 || j == i)
            row.add(1);
        else
            row.add(preRow.get(j-1) + preRow.get(j)); 
    preRow = row;
}
```
* 改进二：只使用一个`List`集合（这里的内循环有点难理解，其实每次循环保存下来的`temp`是第$j$个值，随后赋给`pre`，给下一次$j$循环使用；那么相对于下次的$j$来说，这个`pre`就是第$j-1$个，即它的上一行的前一个值，因此此时可将第$j$个值更新为`precur.get(j)`。）
```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> cur = new ArrayList<>();
        cur.add(1);
        int pre = 1;
        for(int i = 1; i < rowIndex+1; i++){
            for(int j = 1; j < i; j++){
                int temp = cur.get(j);
                cur.set(j, pre + cur.get(j));
                pre = temp;
            } 
            cur.add(1);
        }
        return cur;
    }
}
```
* 改进三：根据上述思路，每次在第$j$个位置的值会被替换，因此要先将之前的值保存下来，如果每一行从后向前更新，就不需要保存了。
```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> cur = new ArrayList<>();
        cur.add(1);
        int pre = 1;
        for(int i = 1; i < rowIndex+1; i++){
            cur.add(1);
            for(int j = i-1; j >= 1; j--)
                cur.set(j, cur.get(j) + cur,get(j-1));
        }
        return cur;
    }
}
```