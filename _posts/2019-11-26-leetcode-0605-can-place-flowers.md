---
layout: post
title: LeetCode 0605 题解
description: "种花问题"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

### 题解
1. 法一：统计每一个连续为0的子数组中可以种花的位置，并相加；特别要注意区分在数组开始为0、在数组末尾为0、在数组开始和末尾均为0的子数组，其种花位置的计算不同。
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int count = 0, sum = 0; 
        int start = 0, end = 0;
        for(int i = 0; i < flowerbed.length; i++){
            while(i < flowerbed.length && flowerbed[i] == 0){
                count++;
                i++;
            }
            start = i-count;
            end = i-1;
            if(start == 0 && end == flowerbed.length-1)
                sum += (count+1)/2;
            else if(start == 0 || end == flowerbed.length-1)
                sum += count/2;
            else
                sum += (count-1)/2;
            count = 0;
        }
        return n <= sum;
    }
}
```

2. 法二（贪心法）：对数组进行扫描，若有连续三个元素均为0，则在中间的元素上种花（即把元素从0改为1），同时考虑到元素在数组开始或末尾时的情况。
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int count = 0; 
        for(int i = 0; i < flowerbed.length; i++){
            if(flowerbed[i] == 0 && (i == 0 || flowerbed[i-1] == 0) && (i == flowerbed.length-1 || flowerbed[i+1] == 0)){
                flowerbed[i] = 1;
                count++;
            }
        }
        return n <= count;
    }
}
```
* 其中，关于`n`的判断可提前，一旦`count >= n`，可提前返回。
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int count = 0; 
        for(int i = 0; i < flowerbed.length; i++){
            if(flowerbed[i] == 0 && (i == 0 || flowerbed[i-1] == 0) && (i == flowerbed.length-1 || flowerbed[i+1] == 0)){
                flowerbed[i] = 1;
                count++;
            }
            if(count >= n)
                return true;
        }
        return false;
    }
}
```
* 或者不使用计数变量，直接用`n`进行控制（不提前返回）。
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        for(int i = 0; i < flowerbed.length; i++){
            if(flowerbed[i] == 0 && (i == 0 || flowerbed[i-1] == 0) && (i == flowerbed.length-1 || flowerbed[i+1] == 0)){
                flowerbed[i] = 1;
                n--;
            }
        }
        return n <= 0;
    }
}
```
* 或者不使用计数变量，直接用`n`进行控制（提前返回）。
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        for(int i = 0; i < flowerbed.length; i++){
            if(flowerbed[i] == 0 && (i == 0 || flowerbed[i-1] == 0) && (i == flowerbed.length-1 || flowerbed[i+1] == 0)){
                flowerbed[i] = 1;
                n--;
            }
            if(n <= 0)
                return true;
        }
        return false;
    }
}
```
3. 法三：在数组两端增加0，这样就不用判定边界条件，只要出现连续三个0，就在中间位置种花。（需额外空间）
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int[] aux = new int[flowerbed.length + 2];
        for(int i = 0; i < flowerbed.length; i++)
            aux[i+1] = flowerbed[i];
        for(int i = 1; i < aux.length-1; i++){
            if(aux[i] == 0 && aux[i-1] == 0 && aux[i+1] == 0){
                aux[i] = 1;
                n--;
            }
        }
        return n <= 0;
    }
}
```
* 通过以下思路可避免额外空间的使用。
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int num = 0, count = 1; 
        //count初始化为1是假定数组起始端增加了一个0，可避免左边的边界问题，仍用每三个0种花的通用方法解决左边边界问题。
        for(int i = 0; i < flowerbed.length; i++){
            if(flowerbed[i] == 0)
                count++;
            else
                count = 0;
            if(count == 3){
                num++;
                count = 1;
            }
        }
        if(count == 2) //跳出循环后右边边界的判定。
            num++;
        return n <= num;
    }
}
```