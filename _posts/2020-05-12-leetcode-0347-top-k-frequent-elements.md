---
layout: post
title: LeetCode 0347 题解
description: "前 K 个高频元素"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

### 题解
1. 法一（小根堆）：使用HashMap统计各元素出现次数，随后遍历该Map，并维护一个根据次数进行排序且大小为k的小根堆，这个小根堆里的元素就是前k个高频元素。
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int x : nums)
            map.put(x, map.getOrDefault(x, 0) + 1);
        PriorityQueue<Integer> q = new PriorityQueue<>(new Comparator<Integer>() {
            // 根据value进行排序
            public int compare(Integer o1, Integer o2) {
                return map.get(o1) - map.get(o2);
            }
        });
        for (int key : map.keySet()) {
            q.add(key);
            if (q.size() > k)
                q.poll();
        }
        int[] res = new int[k];
        int index = 0;
        for (int x : q) {
            res[index++] = x;
        }
        return res;
    }
}
```
2. 法二（桶排序）：创建一个`List<Integer>bucket[]`，遍历Map时将出现次数为`i`的元素放入`bucket[i]`所在的list中。随后从后往前遍历`bucket`，即可将前k个高频元素选出。由于题目说明了答案是唯一的，因此当选取的元素没有达到k个时，可以将当前list的元素全部加入返回值中，不会出现当前桶的元素中一部分加入了返回值而另一部分没有的情况。
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        // 统计出现次数
        for (int x : nums)
            map.put(x, map.getOrDefault(x, 0) + 1);
        List<Integer>[] bucket = new ArrayList[nums.length + 1];
        // 放入桶中
        for (int key : map.keySet()) {
            int cnt = map.get(key);
            if (bucket[cnt] == null)
                bucket[cnt] = new ArrayList<Integer>();
            bucket[cnt].add(key);
        }
        List<Integer> list = new ArrayList<>();
        // 遍历桶数组
        for (int i = bucket.length - 1; i >= 0 && list.size() < k; i--) {
            if (bucket[i] == null)
                continue;
            list.addAll(bucket[i]);
        }
        int[] res = new int[k];
        int index = 0;
        for (int x : list) {
            res[index++] = x;
        }
        return res;
    }
}
```
* 若题目没有说答案唯一，则下面方法将返回其中一个答案。
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        // 统计出现次数
        for (int x : nums)
            map.put(x, map.getOrDefault(x, 0) + 1);
        List<Integer>[] bucket = new ArrayList[nums.length + 1];
        // 放入桶中
        for (int key : map.keySet()) {
            int cnt = map.get(key);
            if (bucket[cnt] == null)
                bucket[cnt] = new ArrayList<Integer>();
            bucket[cnt].add(key);
        }
        List<Integer> list = new ArrayList<>();
        // 遍历桶数组
        for (int i = bucket.length - 1; i >= 0 && list.size() < k; i--) {
            if (bucket[i] == null)
                continue;
            // 若桶中的所有元素可以放入，则全部放入。
            if (bucket[i].size() <= k - list.size())
                list.addAll(bucket[i]);
            // 否则，选择部分元素放入。
            else
                list.subList(0, k - list.size());
        }
        int[] res = new int[k];
        int index = 0;
        for (int x : list) {
            res[index++] = x;
        }
        return res;
    }
}
```