---
layout: post
title: LeetCode 0121 题解
description: "买卖股票的最佳时机"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

### 思路
暴力求解：两重循环进行列举，找到最大差值。

### 题解
```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0;
        for(int i = 0; i < prices.length; i++)
            for(int j = i+1; j < prices.length; j++){
                int profit = prices[j] - prices[i];
                if(profit > max)
                    max = profit;
            }
        return max;
    }
}
```
* 法二：需要求解某一最小值之后的最大差值，则可以更新minPrice（当前最低价格）和maxProfit（当前最大利润）。这样仅需遍历数组一次。
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0) return 0;
        int minPrice = prices[0], maxProfit = 0;
        for(int i = 0; i < prices.length; i++){
            if(prices[i] < minPrice)
                minPrice = prices[i];
            if(maxProfit < prices[i] - minPrice)
            	maxProfit = prices[i] - minPrice;
        }
        return maxProfit;
    }
}
```
* 改进一：将`minPrice`初始化为`Integer.MAX_VALUE`，就可使其读入数组第一个元素时就进行更新，也就省去了数组长度为0的特殊情况判断。
* 改进二：第二个`if`更换为`else if`。因为若一个元素为最小值，此时往下算的`maxProfit`为0，因此换成`else if`可减少判断次数。
```java
class Solution {
    public int maxProfit(int[] prices) {
        // if(prices.length == 0) return 0; 省略
        int minPrice = Integer.MAX_VALUE, maxProfit = 0;
        for(int i = 0; i < prices.length; i++){
            if(prices[i] < minPrice)
                minPrice = prices[i];
            else if(maxProfit < prices[i] - minPrice) //改为else if以减少不必要的判断。
            	maxProfit = prices[i] - minPrice;
        }
        return maxProfit;
    }
}
