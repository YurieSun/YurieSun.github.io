---
layout: post
title: LeetCode 0239 题解
description: "滑动窗口最大值"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

### 题解
1. 法一：使用一个辅助的链表存储滑动窗口中的可能最大值（下标），从队列头部到队列尾部维持从大到小的顺序，这样队列头部第一个值就是窗口内最大值。
```java
class Solution {
        public int[] maxSlidingWindow(int[] nums, int k) {
            if (nums == null || nums.length == 0)
                return new int[0];
            int[] res = new int[nums.length - k + 1];
            LinkedList<Integer> qmax = new LinkedList<>();
            int index = 0;
            for (int i = 0; i < nums.length; i++) {
                // 找到当前下标可以存放的位置，也就是如果队列尾部的数比当前数小，就不断弹出，
                // 直到队列空或出现比当前数更大的数。
                while (!qmax.isEmpty() && nums[qmax.peekLast()] <= nums[i])
                    qmax.pollLast();
                // 当前数入队
                qmax.addLast(i);
                // 如果队列头部的数已经不在窗口中，则需要将这个数出队。
                if (qmax.peekFirst() == i - k)
                    qmax.pollFirst();
                // 当窗口产生后，开始记录最大值。
                if (i >= k - 1)
                    res[index++] = nums[qmax.peekFirst()];
            }
            return res;
        }
    }
```