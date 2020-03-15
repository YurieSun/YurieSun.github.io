---
layout: post
title: 剑指Offer 38 题解
description: "字符串的排列"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

### 题解
1. 法一（回溯）
```java
class Solution {
    List<String> list = new ArrayList<>();
    public String[] permutation(String s) {
        if (s == null || s.length() == 0)
            return new String[0];
        char[] temp = s.toCharArray();
        Arrays.sort(temp);
        backtrcking(temp, new boolean[temp.length], new StringBuilder());
        String[] res = new String[list.size()];
        for (int i = 0; i < res.length; i++)
            res[i] = list.get(i);
        return res;

    }
    private void backtrcking(char[] temp, boolean[] used, StringBuilder path) {
        if (path.length() == temp.length) {
            list.add(path.toString());
            return;
        }
        for (int i = 0; i < temp.length; i++) {
            if (used[i])
                continue;
            // 去重
            if (i != 0 && temp[i] == temp[i - 1] && !used[i - 1])
                continue;
            used[i] = true;
            path.append(temp[i]);
            backtrcking(temp, used, path);
            path.deleteCharAt(path.length() - 1);
            used[i] = false;
        }
    }
}
```
2. 法二：对于一个字符串，先将当前字符与后面的字符交换位置，再递归处理后面的字符，处理完需将字符重新换位置回到初始状态，从而进行后续的字符交换位置。
```java
class Solution {
    Set<String> set = new HashSet<>();
    public String[] permutation(String s) {
        if (s == null || s.length() == 0)
            return new String[0];
        char[] temp = s.toCharArray();
        helper(temp, set, 0);
        String[] res = new String[set.size()];
        int i = 0;
        for (String string : set) {
            res[i++] = string;
        }
        return res;
    }
    private void helper(char[] temp, Set<String> set, int begin) {
        if (begin == temp.length) {
            set.add(String.valueOf(temp));
            return;
        }
        for (int i = begin; i < temp.length; i++) {
            // 当前字符与开头字符交换位置
            char c = temp[begin];
            temp[begin] = temp[i];
            temp[i] = c;
            // 递归处理后面的字符
            helper(temp, set, begin + 1);
            // 将当前字符和开头字符再次交换位置，这一步相当于回溯。
            c = temp[begin];
            temp[begin] = temp[i];
            temp[i] = c;
        }
    }
}
```
* 加上字符数组的排序与剪枝，可加快运行时间。
```java
class Solution {
    Set<String> set = new HashSet<>();

    public String[] permutation(String s) {
        if (s == null | s.length() == 0)
            return new String[0];
        char[] temp = s.toCharArray();
        // 排序
        Arrays.sort(temp);
        helper(temp, set, 0);
        String[] res = new String[set.size()];
        int i = 0;
        for (String string : set) {
            res[i++] = string;
        }
        return res;
    }
    private void helper(char[] temp, Set<String> set, int begin) {
        if (begin == temp.length) {
            set.add(String.valueOf(temp));
            return;
        }
        for (int i = begin; i < temp.length; i++) {
            // 剪枝
            if (i != begin && (temp[i] == temp[i - 1] || temp[i] == temp[begin]))
                continue;
            char c = temp[begin];
            temp[begin] = temp[i];
            temp[i] = c;
            helper(temp, set, begin + 1);
            c = temp[begin];
            temp[begin] = temp[i];
            temp[i] = c;
        }
    }
}
```