---
layout: post
title: LeetCode 0028 题解
description: "实现strStr"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[实现strStr](https://leetcode-cn.com/problems/implement-strstr/)

### 思路
依次扫描`haystack`的每个字符，若与`needle`的第一个字符相同，则进入循环检验`needle`的每个字符，不相等则跳出，完成则返回位置。

### 题解

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        if(needle.length() > haystack.length()) return -1;
        for (int i = 0; i <= haystack.length()-needle.length(); i++) {
            if (haystack.charAt(i) == needle.charAt(0)){
            	int temp = i, j = 0;
            	while (j < needle.length()) {
                    if (haystack.charAt(temp) != needle.charAt(j)) break;
                    else j++;
                    temp++;
                }
            	if (j == needle.length())
            		return i;
            }
        }
        return -1;
    }
}
```

* 问题  
1. 第一个`if`判断中，若条件改为`needle == null`或`nededle == ""`，则无法通过如`"a",""`的用例。
2. 在第一个`for`循环中，若使用`i < haystack.length()`，会出现`String index out of range`的错误。

* 方法二：设置标记以判断循环结束后的情况；不用$temp$储存$i$进入二重循环的初始值。
```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        if(needle.length() > haystack.length()) return -1;
        for (int i = 0; i <= haystack.length()-needle.length(); i++) {
            if (haystack.charAt(i) == needle.charAt(0)){
            	 int j = 0;
                 boolean flag = true;
             	while (j < needle.length()) {
                     if (haystack.charAt(i) != needle.charAt(j)){
                         flag = false;
                         i -= j; //需要恢复进入时i的初始值
                         break;
                     } 
                     j++;
                     i++;
                 }
             	if (flag == true)
             		return i-j;
            }
        }
        return -1;
    }
}
```

* 方法三（执行速度较慢）
```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        if(needle.length() > haystack.length()) return -1;
        int i = 0, j = 0;
        while(i < haystack.length() && j < needle.length()) {
        	if(haystack.charAt(i) == needle.charAt(j)) {
        		i++;
        		j++;
        	}
        	else {
        		i = i - j + 1;
        		j = 0;
        	}
        }
        if(j == needle.length())
        	return i-j;
        return -1;
    }
}
```
* 总结  
以上的暴力解法思路均一致，其关键代码可总结为以下两种写法（《算法》中的代码，非常简洁）：
1. 两重循环
```java
class Solution {
    public int strStr(String haystack, String needle) {
        int N = haystack.length();
        int M = needle.length();
        for(int i = 0; i <= N - M; i++){
            int j; //必须在循环外定义j，否则退出循环后j即消失。
            for(j = 0; j < M; j++)
                if(haystack.charAt(i+j) != needle.charAt(j)) break; 
            if(j == M) return i; //指向haystack第一个位置的指针未变化，因此直接返回i。
        }
        return -1;
    }
}
```
2. 显式回退
```java
class Solution {
    public int strStr(String haystack, String needle) {
        int i, N = haystack.length();
        int j, M = needle.length(); //同理，必须在循环外定义j
        for(i = 0, j = 0; i < N && j <M; i++){
            if(haystack.charAt(i) == needle.charAt(j)) j++; 
            else{
                i -= j;
                j = 0;
            }
        }
        if(j == M) return i-M; //指向haystack第一个位置的指针已变化，因此返回i-M。
        return -1;
    }
}
```

* 改进  
暴力解法所需时间与$M\times N$成正比，其中$M$、$N$分别为两个字符串长度。因此采用KMP算法，可保证线性时间内完成，但使用了额外的空间。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if(needle.length() == 0) return 0;
        if(needle.length() > haystack.length()) return -1;
        int N = haystack.length();
        int M = needle.length();
        int[][] dfa = new int[256][M];
    	dfa[needle.charAt(0)][0] = 1;
    	for (int X = 0, j = 1; j < M; j++) {
    		for (int c = 0; c < 256; c++)
    			dfa[c][j] = dfa[c][X];
    		dfa[needle.charAt(j)][j] = j+1;
    		X = dfa[needle.charAt(j)][X];
    	}
    	int i, j;
        for (i = 0,j = 0; i < N && j < M; i++) 
        	j = dfa[haystack.charAt(i)][j];
        if(j == M) return i-M;
        return -1;
    }
}
```

### 思考
在LeetCode上运行KMP算法，所用时间与内存均大于暴力算法，这与LeetCode的测试用例有关。KMP算法的优势在于不用回退`haystack`指针，但计算`dfa[][]`数组需要额外的空间，且较适用于`needle`中重复前缀较多的情况。
