---
layout: post
title: LeetCode 0009 题解
description: "回文数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[回文数](https://leetcode-cn.com/problems/palindrome-number/)

### 题解
1. 法一：将整数反转，判断反转前后的数字是否相等。
```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0)
            return false;
        int temp = x;
        int reverse = 0;
        while(temp != 0){
            reverse = reverse * 10 + temp % 10;
            temp /= 10;
        }
        return x == reverse;
    }
}
```
* 改进：上述方法中将整数完全反转会有溢出的问题（虽然溢出肯定不是回文数），并且将整个数进行反转，耗费时间较长。因此可以考虑将数字的右边一半进行反转。
```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x == 0)
            return true;
        if(x < 0 || x % 10 == 0)
            return false;
        int right = 0;
        while(x > right){
            right = right * 10 + x % 10;
            x /= 10;
        }
        //若位数是偶数，则正好平分，判断两数是否相等即可；
        //若位数是奇数，则x的位数比要right少一位，因此需判断right舍掉最后一位的数是否和x相等。
        return x == right || x == right / 10;
    }
}
```
2. 法二：对于一个数字，每次取对称位置的数进行比较，只要不同就可以返回。
```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0)
            return false;
        int div = 1;
        while(x / div >= 10)
            div *= 10;
        while(x > 0){
            int left = x / div;
            int right = x % 10;
            if(left != right)
                return false;
            x = (x % div) / 10;
            div /= 100;
        }
        return true;
    }
}
```
3. 法三：将数字转换成字符串进行判断。
```java
class Solution {
    public boolean isPalindrome(int x) {
        String reverse = (new StringBuilder(x + "")).reverse().toString();
        return (x + "").equals(reverse);
    }
}
```