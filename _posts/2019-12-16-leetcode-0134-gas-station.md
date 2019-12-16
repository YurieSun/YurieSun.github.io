---
layout: post
title: LeetCode 0134 题解
description: "加油站"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[加油站](https://leetcode-cn.com/problems/gas-station/)

### 题解
1. 法一（暴力两重循环）：从第0个加油站开始，若该加油站满足到达下一站的条件，则进入循环判断能否完成绕路行驶回到起点，若可以则返回起点，否则，判断下一个点，直到将所有点作为起点的情况判断完毕。（运行太慢）
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int len = gas.length, i = 0;
        while(i < len){
            if(gas[i] >= cost[i]){
                int j = i;
                int sum = gas[j];
                do{
                    sum += gas[(j+1)%len] - cost[j%len];
                    j = (j+1)%len;
                    if(sum < cost[j%len])
                        break;
                    
                }while(i != j);
                if(i == j)
                    return i; 
            }
            i++;
        }
        return -1;
    }
}
```
2. 法二：（其实就是求最大子序和）这个算法基于两个认识：(1)若汽油总量$\ge$消耗总量即`total >= 0`，则一定有环绕一圈的解；(2)若从$0$开始，到$i$时无法继续前进，这意味着从$0$到$i$的任何一个站都不可能作为起始站。那么当我们找到最大汽油量（即最大子列和）的开始位置，就必定能够覆盖其它出现负数的加油站（前提是`total >= 0`），使得其顺利环绕一圈。因此，这道题就是最大子列和的包装。（妙）
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int total = 0, sum = 0, start = 0;
        for(int i = 0; i < gas.length; i++){
            total += gas[i] - cost[i];
            if(sum > 0)
                sum += gas[i] - cost[i];
            else{
                sum = gas[i] - cost[i];
                start = i;
            }     
        }
        return total >= 0 ? start : -1;
    }
}
```