---
layout: post
title: LeetCode 0022 题解
description: "括号生成"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

### 题解
1. 法一（DFS）：`left`和`right`分别表示未使用的左括号和右括号的数量。当左括号和右括号都是用完之后，将得到的字符串添加进结果集。剪枝是为了保证生成的括号是有效的，当`left > right`时，说明右括号使用的更多，这种情况就会使括号无效，因为多出来的右括号没有相对应的左括号。
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        if (n <= 0){
            return res;
        }
        dfs(n, n, "", res);
        return res;
    }
    private void dfs(int left, int right, String cur, List<String> list) {
        if (left == 0 && right == 0) {
            list.add(cur);
            return;
        }
        // 剪枝
        if (left > right) {
            return;
        }
        if (left > 0) {
            dfs(left - 1, right, cur + "(", list);
        }
        if (right > 0) {
            dfs(left, right - 1, cur + ")", list);
        }
    }
}
```
* 使用BFS也可以，但是比DFS要复杂。
2. 法二（动态规划）：`dp[i]`表示使用`i`对括号生成的括号集合，这里使用`List`集合。使用`i-1`对括号的结果为`dp[i-1]`，则加一对括号后得到`dp[i]`。有效括号肯定是以左括号开头，因此将这一对新增的括号分成括号内和右边的括号外两部分，则这两部分的括号对数之和为`i-1`，因此，状态转移方程为`dp[i]中的一个结果="("+dp[j]中的一个结果+")"+dp[i-1-j]中的一个结果`，其中`j`的取值为`0`到`i-1`。
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        if (n <= 0) {
            return new ArrayList<>();
        }
        List<List<String>> dp = new ArrayList<>();
        List<String> list = new ArrayList<>();
        list.add("");
        dp.add(list);
        for (int i = 1; i <= n; i++) {
            list = new ArrayList<>();
            for (int j = 0; j <= i - 1; j++) {
                List<String> list1 = dp.get(j);
                List<String> list2 = dp.get(i - 1 - j);
                for (String s1 : list1) {
                    for (String s2 : list2) {
                        list.add("(" + s1 + ")" + s2);
                    }
                }
            }
            dp.add(list);
        }
        return dp.get(n);
    }
}
```