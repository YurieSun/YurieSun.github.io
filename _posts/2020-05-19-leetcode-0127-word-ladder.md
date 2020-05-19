---
layout: post
title: LeetCode 0127 题解
description: "单词接龙"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[单词接龙](https://leetcode-cn.com/problems/word-ladder/)

### 题解
1. 法一（BFS）
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        Queue<String> q = new LinkedList<>();
        Set<String> visited = new HashSet<>();
        q.add(beginWord);
        visited.add(beginWord);
        int step = 0;
        while (!q.isEmpty()) {
            int size = q.size();
            step++;
            for (int i = 0; i < size; i++) {
                String cur = q.poll();
                if (cur.equals(endWord))
                    return step;
                for (String s : wordList) {
                    if (count(s, cur) && !visited.contains(s)) {
                        q.add(s);
                        visited.add(s);
                    }
                }
            }
        }
        return 0;
    }
    // 判断能否将s变成t
    private boolean count(String s, String t) {
        int res = 0;
        for (int i = 0; i < s.length() && res <= 1; i++) {
            if (s.charAt(i) != t.charAt(i))
                res++;
        }
        return res == 1;
    }
}
```
* 优化：可以使用数组代替`HashSet`来记录已经访问的节点。
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        Queue<String> q = new LinkedList<>();
        boolean[] visited = new boolean[wordList.size()];
        q.add(beginWord);
        int index = wordList.indexOf(beginWord);
        if (index != -1){
            visited[index] = true;
            return 1;
        }
        int step = 0;
        while (!q.isEmpty()) {
            int size = q.size();
            step++;
            for (int i = 0; i < size; i++) {
                String cur = q.poll();
                if (cur.equals(endWord))
                    return step;
                for (int j = 0; j < wordList.size(); j++) {
                    String s = wordList.get(j);
                    if (!visited[j] && count(s, cur)) {
                        q.add(s);
                        visited[j] = true;
                    }
                }
            }
        }
        return 0;
    }
    private boolean count(String s, String t) {
        int res = 0;
        for (int i = 0; i < s.length() && res <= 1; i++) {
            if (s.charAt(i) != t.charAt(i))
                res++;
        }
        return res == 1;
    }
}
```
* 使用双向BFS进行优化（在已知终点的情况下可以这么优化）。
```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        // q1从起点开始遍历
        Set<String> q1 = new HashSet<>();
        // q2从终点开始遍历
        Set<String> q2 = new HashSet<>();
        Set<String> visited = new HashSet<>();
        q1.add(beginWord);
        q2.add(endWord);
        int step = 0;
        while (!q1.isEmpty() && !q2.isEmpty()) {
            step++;
            // HashSet在遍历过程中不能添加元素，因此定义一个temp。
            HashSet<String> temp = new HashSet<>();
            for (String cur : q1) {
                // 若从起点开始与从终点开始有交集，可以返回。
                if (q2.contains(cur))
                    return step;
                visited.add(cur);
                for (String s : wordList) {
                    if (!visited.contains(s) && count(s, cur)) {
                        temp.add(s);
                    }
                }
            }
            // temp相当于q1的下一层，当q1遍历完成后，里面的节点不再需要了，
            // 则将q2赋值给q1，下次相当于是从终点开始扩散。而temp赋给q1，等待继续被遍历。
            q1 = q2;
            q2 = temp;
        }
        return 0;
    }
    private boolean count(String s, String t) {
        int res = 0;
        for (int i = 0; i < s.length() && res <= 1; i++) {
            if (s.charAt(i) != t.charAt(i))
                res++;
        }
        return res == 1;
    }
}
```