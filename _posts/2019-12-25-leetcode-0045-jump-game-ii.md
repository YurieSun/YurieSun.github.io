---
layout: post
title: LeetCode 0045 题解
description: "跳跃游戏 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

### 题解
1. 法一（贪心）：遍历数组，更新经过每个元素时可以到达的最远位置（从当前位置到最远位置都可以作为起跳点，不断更新最远位置就是在找从哪个点起跳可以到达最远）；当经过这个最大位置时，说明这是跳跃一步后可以到达的最远位置（终点），此时更新终点，并增加步数。（`for`循环的结束位置是`nums.length - 1`，这是由于在第一个位置时多加了一步。）
```java
class Solution {
    public int jump(int[] nums) {
        int end = 0, maxPosition = 0;
        int step = 0;
        for(int i = 0; i < nums.length - 1; i++){
            maxPosition = Math.max(maxPosition, i + nums[i]);
            if(i == end){
                end = maxPosition;
                step++;
            }
                
        }
        return step;
    }
}
```
2. 法二：从后往前遍历，找到前面位置里能到达当前位置的最小位置，同时更新位置，增加步数。
```java
class Solution {
    public int jump(int[] nums) {
        int position = nums.length - 1;
        int step = 0;
        while(position != 0){
            for(int i = 0; i < position; i++)
                if(nums[i] + i >= position){
                    position = i;
                    step++;
                    break;
                }
        }
        return step;
    }
}
```
