---
layout: post
title: LeetCode 0849 题解
description: "到最近的人的最大距离"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[到最近的人的最大距离](https://leetcode-cn.com/problems/maximize-distance-to-closest-person/)

### 题解
1. 法一：出现0时进入循环统计0的个数`count`。若该序列出现在数组开头或末尾，则其最大距离为`count`；若出现在数组中间，则最大距离为`(count+1)/2`。同时更新最大距离，在跳出`if`后，将`count`归零，直至扫描数组结束。
```java
class Solution {
    public int maxDistToClosest(int[] seats) {
        int count = 0, max = 0;
        for(int i = 0; i < seats.length; i++){
            if(seats[i] == 0){
                while(i < seats.length && seats[i] == 0){
                    count++;
                    i++;
                }
                if(i -count != 0 && i != seats.length)
                    count = (count+1)/2;
                if(count > max)
                    max = count;
            }
            count = 0;
        }
        return max;
    }
}
```
2. 法二：记录1出现的位置`start`，并更新最大距离。其初始位置设为-1，并且要处理数组开头与末尾出现0时计算最大距离方法不同的情况。
```java
class Solution {
    public int maxDistToClosest(int[] seats) {
        int start = -1, max = 0;
        for(int i = 0; i < seats.length; i++){
            if(seats[i] == 1){
                if(start == -1) // 若数组开头出现0
                    max = i;
                else
                    max = Math.max(max, (i-start)/2);
                start = i;
            }
        }
        //若数组末尾不是0，则seats.length - 1 - start的计算值为0；如果是0，则要根据末尾的距离计算方法更新最大值。
        max = Math.max(max, seats.length - 1 - start); 
        return max;
    }
}
```
3. 法三：分别找到左边第一个1的位置和右边第一个1的位置，并取两者中较大的距离；在这两个1中间的范围找最大距离，最终返回最大距离。
```java
class Solution {
    public int maxDistToClosest(int[] seats) {
        int midCount = 0;
        int left = 0, right = seats.length-1;
        // 左边第一个1的位置
        while(seats[left] == 0)
            left++;
        // 右边第一个1的位置
        while(seats[right] == 0)
            right--;
        int max = Math.max(left, seats.length-1-right);
        for(int i = left+1; i < right; i++){
            if(seats[i] == 0)
                midCount++;
            else
                midCount = 0;
            max = Math.max(max, (midCount+1)/2);
        }
        return max;
    }
}
```