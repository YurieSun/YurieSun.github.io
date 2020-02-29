---
layout: post
title: LeetCode 0343 题解
description: "整数拆分"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[整数拆分](https://leetcode-cn.com/problems/integer-break/)

### 题解
1. 法一（数学）：根据前几个数字的拆分分析（参考 [整数拆分（贪心，清晰图表解析](https://leetcode-cn.com/problems/integer-break/solution/343-zheng-shu-chai-fen-tan-xin-by-jyd/)）和数学推导（参考 [整数拆分 - 数学方法（含完整推导）](https://leetcode-cn.com/problems/integer-break/solution/zheng-shu-chai-fen-shu-xue-fang-fa-han-wan-zheng-t/)）可知，需要将数字`n`分成尽可能多的`3`；若余数为`0`，则恰好分完；若余数为`1`，则抽取一个`3`，并和`1`分为`2+2`；若余数为`2`，则该种分法也可以。根据上述方法，将`n`分完后，计算对应的乘积即可。
```java
class Solution {
    public int integerBreak(int n) {
        // 由于数字必须分成至少两个数，因此对于2和3，必须分为1+1和1+2。
        if(n <= 3)
            return n - 1;
        int a = n / 3;
        int b = n % 3;
        if(b == 0)
            return (int)Math.pow(3, a);
        if(b == 1)
            return (int)Math.pow(3, a - 1) * 4;
        return (int)Math.pow(3, a) * 2;
    }
}
```
2. 法二（记忆化+暴力搜索）：暴力搜索对于`n`的每一种分解方案，并更新最大值。但不断递归的过程中会出现大量重复计算，因此可以用备忘录的方法进行记忆化；如果已经计算过，就直接将值返回。这是自顶向下的方法。
```java
class Solution {
    int[] memo;
    public int integerBreak(int n) {
        memo = new int[n + 1];
        return helper(n);
    }
    private int helper(int n){
        if(n == 2)
            return 1;
        int res = 0;
        if(memo[n] != 0)
            return memo[n];
        for(int i = 1; i < n; i++)
            res = Math.max(res, Math.max(i * (n-i), i * helper(n-i)));
        memo[n] = res;
        return res;
    }
}
```
3. 法三（动态规划）：这是自底向上的方法，通过前面已经计算过的值，计算当前值。
```java
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n+1];
        dp[2] = 1;
        for(int i = 3; i <= n; i++)
            for(int j = 1; j <= i - 1; j++)
                dp[i] = Math.max(dp[i], Math.max(j * (i-j), j * dp[i-j]));
        return dp[n];
    }
}
```
