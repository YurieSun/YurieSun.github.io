---
layout: post
title: LeetCode 0295 题解
description: "数据流的中位数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)

### 题解
1. 法一（排序）：每次需要输出中位数时，对数组进行排序。时间复杂度为 $O(n\lg n)$。
```java
class MedianFinder {
    private List<Integer> list;
    //initialize your data structure here.
    public MedianFinder() {
        list = new ArrayList<>();
    }
    public void addNum(int num) {
        list.add(num);
    }
    public double findMedian() {
        Collections.sort(list);
        int N = list.size();
        if ((N & 1) == 0)
            return (double) (list.get((N - 2) / 2) + list.get(N / 2)) / 2;
        else
            return (double) list.get((N - 1) / 2);
    }
}
```
2. 法二（二分）：每次插入元素时，需要保持数组的有序性。通过二分法找到元素应该插入的位置，并将其插入。由于在数组中插入元素需要移动该位置及其后面的元素，因此时间复杂度为 $O(n)$。
```java
class MedianFinder {
    private List<Integer> list;
    //initialize your data structure here.
    public MedianFinder() {
        list = new ArrayList<>();
    }
    public void addNum(int num) {
        int lo = 0, hi = list.size();
        int cut = 0;
        while (lo < hi) {
            int mid = lo + (hi - lo) / 2;
            int temp = list.get(mid);
            if (num == temp) {
                cut = mid;
                break;
            }
            if (num > temp)
                lo = mid + 1;
            else
                hi = mid;
        }
        if (lo == hi)
            cut = lo;
        list.add(cut, num);
    }
    public double findMedian() {
        int N = list.size();
        if ((N & 1) == 0)
            return (double) (list.get((N - 2) / 2) + list.get(N / 2)) / 2;
        else
            return (double) list.get((N - 1) / 2);
    }
}
```
3. 法三（优先队列）：通过大顶堆和小顶堆分别存储元素的左有序部分和右有序部分，若元数个数为奇数，则让大顶堆的元素多一个。这样，若元素个数为奇数，则中位数是大顶堆的堆顶元素；若元素个数为偶数，则中位数是大顶堆的堆顶元素与小顶堆的堆顶元素的平均值。因此，每次插入元素时，需要维护这两个优先队列，时间复杂度为 $O(lg n)$。
```java
class MedianFinder {
    private PriorityQueue<Integer> maxheap;
    private PriorityQueue<Integer> minheap;
    private int size;
    //initialize your data structure here.
    public MedianFinder() {
        // PriorityQueue默认是小顶堆，这里通过lamda表达式将其定义为大顶堆。
        maxheap = new PriorityQueue<>((x, y) -> y - x);
        minheap = new PriorityQueue<>();
        size = 0;
    }
    public void addNum(int num) {
        size++;
        maxheap.add(num);
        minheap.add(maxheap.poll());
        if ((size & 1) != 0)
            maxheap.add(minheap.poll());
    }
    public double findMedian() {
        if ((size & 1) == 0)
            return (double) (maxheap.peek() + minheap.peek()) / 2;
        else
            return (double) maxheap.peek();
    }
}
```