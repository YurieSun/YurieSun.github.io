---
layout: post
title: LeetCode 0274 题解
description: "H指数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[H指数](https://leetcode-cn.com/problems/h-index/)

### 题解
1. 法一（比较排序法）：数组排序后，对其从后往前遍历，`N-i`为当前所遍历过的文章数量，若当前被引次数`citations[i]`大于`N-i`，则此时的文章数量时满足H指数定义，一旦不满足该条件后，则该文章数量不满足H指数定义。
```java
class Solution {
    public int hIndex(int[] citations) {
        if(citations == null || citations.length == 0)
            return 0;
        int N = citations.length, h = 0; 
        Arrays.sort(citations);
        for(int i = N-1; i >= 0; i--){
            if(citations[i] > N-i || citations[i] == N-i)
                h++; // h = N - i;
        }
        return h;
    }
}
```
2. 法二（计数排序法）：因为数组长度`N`即文章总数，因此H指数不可能超过数组长度，则被引次数超过`N`的元素可将其等价为`N`。将数组预处理后，新建数组来存储各被引次数的文章数量；再次遍历数组`num`得到各被引次数大于等于`i`的文章数量。再次遍历，找到符合条件的最大的`i`即为H指数。
```java
class Solution {
    public int hIndex(int[] citations) {
        if(citations == null || citations.length == 0)
            return 0;
        int N = citations.length, h = 0; 
        int[] num = new int[N+1];
        // 将被引次数超过N的等价为N，同时统计各被引次数对应的文章数。
        for(int i = 0; i < N; i++){
            if(citations[i] > N)
                citations[i] = N;
            num[citations[i]]++;
        }
        // num[i]表示被引次数大于等于i的文章数。
        for(int i = N; i > 0; i--)
            num[i-1] += num[i];
        // 找到符合条件的最大的i。
        for(int i = 0; i < N+1; i++)
            if(num[i] > i || num[i] == i)
                h = i;
        return h;
    }
}
```
* 以上使用了三次遍历，可采用更简洁的写法，且只需两次遍历。
```java
class Solution {
    public int hIndex(int[] citations) {
        if(citations == null || citations.length == 0)
            return 0;
        int N = citations.length; 
        int[] num = new int[N+1];
        for(int c : citations)
            num[Math.min(c, N)]++;
        int k = N;
        for(int i = num[k]; i < k; i += num[k])
            k--;
        return k;
    }
}
```

### 思考
1. 比较排序的最低时间复杂度为$O(n\lg n)$，不使用额外空间；而计数排序的时间复杂度为$O(n)$，但需要$O(n)$的额外空间。