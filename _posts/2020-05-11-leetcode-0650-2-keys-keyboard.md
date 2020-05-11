---
layout: post
title: LeetCode 0650 题解
description: "只有两个键的键盘"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[只有两个键的键盘](https://leetcode-cn.com/problems/2-keys-keyboard/)

### 题解
1. 法一：本质上是将`n`分解成素数的乘积，最终返回这些素数的和即可。[只有两个键的键盘](https://leetcode-cn.com/problems/2-keys-keyboard/solution/zhi-you-liang-ge-jian-de-jian-pan-by-leetcode/) 解释的很清楚。
```java
class Solution {
    public int minSteps(int n) {
        int res = 0, d = 2;
        while (n > 1) {
            // 若当前素数是n的因子，则不断累加。
            while (n % d == 0) {
                res += d;
                n /= d;
            }
            // 若不是，则继续寻找下一个素数因子。
            d++;
        }
        return res;
    }
}
```
* 采用递归
```java
class Solution {
    public int minSteps(int n) {
        if (n == 1)
            return 0;
        for (int i = 2; i <= Math.sqrt(n); i++) {
            if (n % i == 0)
                return i + minSteps(n / i);
        }
        // 无法分解，n就是一个素数。
        return n;
    }
}
```
* 采用动态规划：`dp[i]`表示得到`i`所需的最小步数。
```java
class Solution {
    public int minSteps(int n) {
        int[] dp = new int[n + 1];
        for (int i = 2; i <= n; i++) {
            // 初始值为不能分解的情况下的值。
            dp[i] = i;
            for (int j = 2; j <= Math.sqrt(n); j++) {
                // 若可以进行分解，则进行分解并覆盖初始值。
                if (i % j == 0) {
                    dp[i] = dp[j] + dp[i / j];
                    break;
                }
            }
        }
        return dp[n];
    }
}
```
