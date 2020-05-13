---
layout: post
title: LeetCode 0451 题解
description: "根据字符出现频率排序"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[根据字符出现频率排序](https://leetcode-cn.com/problems/sort-characters-by-frequency/)

### 题解
1. 法一（大根堆）：使用HashMap统计各元素出现次数，随后遍历该Map，并维护一个根据次数进行排序的大根堆。再不断弹出堆顶元素，并将对应次数的字符进行拼接。
```java
class Solution {
    public String frequencySort(String s) {
        if (s == null || s.length() == 0)
            return s;
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++)
            map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);
        PriorityQueue<Character> q = new PriorityQueue<>(new Comparator<Character>() {
            public int compare(Character a, Character b) {
                return map.get(b) - map.get(a);
            }
        });
        for (char c : map.keySet()) {
            q.add(c);
        }
        StringBuilder sb = new StringBuilder();
        while (!q.isEmpty()) {
            char cur = q.poll();
            int cnt = map.get(cur);
            for (int i = cnt; i > 0; i--)
                sb.append(cur);
        }
        return sb.toString();
    }
}
```
2. 法二（桶排序）：创建一个`List<Integer>cnt[]`，遍历Map时将出现次数为`i`的元素放入`cnt[i]`所在的list中。随后从后往前遍历`cnt`，并将对应次数的字符进行拼接。
```java
class Solution {
    public String frequencySort(String s) {
        if (s == null || s.length() == 0)
            return s;
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++)
            map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);
        ArrayList<Character>[] cnt = new ArrayList[s.length() + 1];
        for (char c : map.keySet()) {
            int value = map.get(c);
            if (cnt[value] == null)
                cnt[value] = new ArrayList<Character>();
            cnt[value].add(c);
        }
        StringBuilder sb = new StringBuilder();
        for (int i = cnt.length - 1; i >= 0; i--) {
            if (cnt[i] == null)
                continue;
            for (char c : cnt[i])
                for (int j = i; j > 0; j--)
                    sb.append(c);
        }
        return sb.toString();
    }
}
```