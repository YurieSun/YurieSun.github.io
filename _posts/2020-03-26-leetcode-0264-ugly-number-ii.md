---
layout: post
title: LeetCode 264 题解
description: "丑数 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)

### 题解
1. 法一（暴力）：从`1`开始判断每个数是否为丑数，直到找到第`n`个。（超时）
2. 法二（最小堆）：从第一个丑数开始，依次乘上2、3、5得到新的丑数，并放入堆中。选择优先队列的原因是，每次需要拿到未统计的最小丑数进行计数，直到找出第`n`个。同时，要注意集合中元素的去重。
```java
class Solution {
    public int nthUglyNumber(int n) {
        Queue<Long> q = new PriorityQueue<>();
        //改成long是为了防止溢出
        long res = 1;
        q.add(res);
        while (n > 0) {
            res = q.poll();
            while (!q.isEmpty() && q.peek() == res)
                q.poll();
            n--;
            q.add(res * 2);
            q.add(res * 3);
            q.add(res * 5);
        }
        return (int) res;
    }
}
```
3. 法三（三指针）：三个指针`p2`、`p3`、`p5`分别表示目前可以和`2`、`3`、`5`相乘的最小数，从中选出最小的作为下一个丑数。同时，如果新计算的丑数可由这三个指针分别计算，则需要右移相应的指针，表示目前指针所指的数已经不能再与其对应的数相乘，这样也达到了去重的目的。
```java
class Solution {
    public int nthUglyNumber(int n) {
        int p2 = 0, p3 = 0, p5 = 0;
        int[] dp = new int[n];
        dp[0] = 1;
        for (int i = 1; i < n; i++) {
            dp[i] = Math.min(Math.min(dp[p2] * 2, dp[p3] * 3), dp[p5] * 5);
            if (dp[i] == dp[p2] * 2)
                p2++;
            if (dp[i] == dp[p3] * 3)
                p3++;
            if (dp[i] == dp[p5] * 5)
                p5++;
        }
        return dp[n - 1];
    }
}
```