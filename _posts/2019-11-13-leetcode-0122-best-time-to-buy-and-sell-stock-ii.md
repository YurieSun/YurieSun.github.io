---
layout: post
title: LeetCode 0122 题解
description: "买卖股票的最佳时机 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

### 思路
根据第121题的思路，将每一个上升区间的谷值和峰值找出来，它们的差值就是一次投资的利润，则将所有利润相加为最大利润。

### 题解
```java
class Solution {
    public int maxProfit(int[] prices) {
        int valley = 0, peak = 0, sum = 0, i = 1;
        while(i <prices.length) {
        	while(i < prices.length && prices[i-1] >= prices[i]) //递减
        		i++;
    		valley = prices[i-1];
        	while(i < prices.length && prices[i-1] <= prices[i]) //递增
        		i++;
    		peak = prices[i-1];
        	sum += peak - valley;
        }
        return sum;
    }
}
```
* 法二：只要是递增的情况，就把当前值减去上一个值作为利润，加到总利润上。（贪心算法）
```java
class Solution {
    public int maxProfit(int[] prices) {
        int sum = 0;
        for(int i = 1; i < prices.length; i++){
            if(prices[i-1] > prices[i])
                sum += prices[i] - prices[i-1];
        }
        return sum;
    }
}
```
上述代码也可写为：
```java
class Solution {
    public int maxProfit(int[] prices) {
        int sum = 0;
        for(int i = 1; i < prices.length; i++){
            int temp = prices[i] - prices[i-1];
            if(temp > 0)
                sum += temp;
        }
        return sum;
    }
}
```
* 法三：动态规划。可参考[暴力搜索 + 贪心算法 + 动态规划](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/solution/tan-xin-suan-fa-by-liweiwei1419-2/)，写得非常详细。