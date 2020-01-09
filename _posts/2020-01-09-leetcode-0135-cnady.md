---
layout: post
title: LeetCode 0135 题解
description: "分发糖果"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[分发糖果](https://leetcode-cn.com/problems/candy/)

### 题解
1. 法一（两次遍历）：使用一个数组`cnady`存储每个小朋友需要的糖果。第一次从左往右遍历`ratings`数组，若当前元素`ratings[i]`比前一个元素`ratings[i-1]`大，则其糖果数`candy[i]`更新为前一个糖果数加1即`candy[i-1]+1`，这样就可以满足左邻居的条件。第二次从右往左遍历`ratings`数组，若当前元素`ratings[i]`比后一个元素`ratings[i+1]`大，且其糖果数`candy[i]`小于等于`candy[i+1]`，则其糖果数`candy[i]`更新为后一个糖果数加1即`candy[i+1]+1`，这样就可以满足右邻居的条件。此时，将数组`candy`中的元素相加，并加上数组长度（相当于每个小朋友初始化时有一个糖），即为糖果总数。
```java
class Solution {
    public int candy(int[] ratings) {
        int sum  = ratings.length;
        int[] candy = new int[ratings.length];
        for(int i = 1; i < ratings.length; i++){
            if(ratings[i] > ratings[i-1] && candy[i] <= candy[i-1])
                candy[i] = candy[i-1] + 1;
        }
        for(int i = ratings.length-2; i >= 0; i--){
            if(ratings[i] > ratings[i+1] && candy[i] <= candy[i+1])
                candy[i] = candy[i+1] + 1;
        }
        for(int n : candy)
            sum += n;
        return sum;
    }
}
```
   * 稍微优化一下：第一次遍历时，由于数组`candy`初始化时每个元素时相等的，因此后面的糖果数总是小于等于前面的糖果数，即`if`条件中的第二个条件总是成立，因此可以简化`if`条件；第三次遍历求和可以放到数组`ratings`的第二次遍历中。
   ```java
   class Solution {
        public int candy(int[] ratings) {
            int[] candy = new int[ratings.length];
            for(int i = 1; i < ratings.length; i++){
                if(ratings[i] > ratings[i-1])
                    candy[i] = candy[i-1] + 1;
            }
            int sum  = ratings.length + candy[candy.length-1]; 
            for(int i = ratings.length-2; i >= 0; i--){
                if(ratings[i] > ratings[i+1])
                    candy[i] = Math.max(candy[i], candy[i+1] + 1);
                sum += candy[i];
            }
            return sum;
        }
    }
   ```
2. 法二：只遍历一次数组且空间复杂度为 $O(1)$ 的方法。数组中每一个谷值的糖果数都应为1，这样就可以确定每一个山坡的上升区间与下降区间的糖果数，而峰值则需要取【从上升区间得到的峰值】和【从下降区间得到的峰值】的较大值。因此，对于上升区间，遍历时依次加入糖果数；对于下降区间，则统计下降点的个数，并根据等差数列求和公式得到下降区间的糖果总数，同时比较两个区间的峰值，并取较大值。
```java
class Solution {
    public int candy(int[] ratings) {
        int sum = 1, pre = 1, countDown = 0;
        for(int i = 1; i < ratings.length; i++){
            // 上升区间
            if(ratings[i] >= ratings[i-1]){
                // 前一个点是谷值
                if(countDown > 0){
                    // countDown是谷值到峰值前一个点的点个数，
                    // 则根据求和公式可得到谷值到峰值前一个点的糖果总和。
                    sum += countDown * (countDown + 1) / 2;
                    // 把峰值加上，这里需要比较上升区间的峰值与从谷底爬上来的值，哪个大就取哪个；
                    // 而位于上升区间时，每个值都加入了sum，包括其峰值，因此需要将其减去。
                    sum += Math.max(pre, countDown + 1) - pre;
                    pre = 1;
                    countDown = 0;
                }
                // 如果和前一个值相等则取1，否则要比前一个糖果数多1。
                pre = (ratings[i-1] == ratings[i]) ? 1 : pre + 1;
                sum += pre;
            }
            else
                countDown++;
        }
        // 在上面的循环中，都是在上升区间将下降区间的糖果数加入，
        // 若rating数组以下降区间结尾，则该区间的糖果数无法计算，
        // 因此在循环结束后，需要再次计算末尾下降区间的糖果数。
        if(countDown > 0){
            sum += countDown * (countDown + 1) / 2;
            sum += Math.max(pre, countDown + 1) - pre;
        }
        return sum;
    }
}
```