---
layout: post
title: LeetCode 0378 题解
description: "有序矩阵中第K小的元素"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

### 题解
1. 法一：根据矩阵元素的排序特点，可将每一行看成一个有序数组，则将问题转化为“求n个有序数组中的第k小的元素”，与“合并k个链表”的思路类似。首先将每一行的第一个元素放入小根堆中，随后从堆中弹出最小值，并开始计数；同时，若弹出值所在行中，在它后面还有元素，则将该元素放入堆中，直到堆中弹出了k个值。堆中存放的数组，除了元素的值，还有行号和列号，这是为了定位这个值的位置，以便能找到它后面的元素。
```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<int[]> pq = new PriorityQueue<>(new Comparator<int[]>() {
            public int compare(int[] o1, int[] o2) {
                return o1[0] - o2[0];
            }
        });
        for (int i = 0; i < matrix.length; i++) {
            pq.offer(new int[]{matrix[i][0], i, 0});
        }
        for (int t = 0; t < k - 1; t++) {
            int[] cur = pq.poll();
            int i = cur[1], j = cur[2];
            if (j + 1 < matrix[0].length) {
                pq.add(new int[]{matrix[i][j + 1], i, j + 1});
            }
        }
        return pq.poll()[0];
    }
}
```
2. 法二（二分查找）：对于这个特殊排列的矩阵，如果以左下角为起点，并选择一个值作为分界点，可将矩阵分为左、右两部分，它们的值分别满足小于等于该分界点和大于该分界点。在将矩阵分成两部分的同时，可以统计左半部分的元素个数。如果大于等于k，说明第k小的数在左半部分，因此缩小右边界；如果小于k，说明第k小的元素在右半部分，因此增大左边界；直到left=right，则正好找到第k小的数。
```java
class Solution {
    // 二分
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0], right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (check(matrix, mid, k, n)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
    // 给定分界点，将矩阵分成两部分，并统计左半部分的元素个数。
    public boolean check(int[][] matrix, int mid, int k, int n) {
        int i = n - 1, j = 0;
        int res = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] <= mid) {
                res += i + 1;
                j++;
            } else {
                i--;
            }
        }
        return res >= k;
    }
}
```