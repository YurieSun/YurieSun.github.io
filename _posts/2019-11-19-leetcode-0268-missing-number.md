---
layout: post
title: LeetCode 0268 题解
description: "缺失数字"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[缺失数字](https://leetcode-cn.com/problems/missing-number/)

### 题解
1. 法一（暴力法）：对于每一个`k`，依次扫描数组，若扫描结束，仍未发现该元素，则返回该元素。
```java
class Solution {
    public int missingNumber(int[] nums) {
        for(int i = 0; i <= nums.length; i++){
            int j = 0;
            while(j < nums.length && nums[j] != i) //循环条件中j的判断一定要放在前面，以防下标溢出。
                j++;
            if(j == nums.length)
                return i;
        }    
        return -1;
    }
}
```

2. 法二（排序法）：先对数组排序再进行扫描，有序之后若元素与所在位置下标相等，则说明该元素存在，不相等则不存在；若扫描结束仍未找到，说明缺失的是最后一个数字$n$。
```java
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        for(int i = 0; i < nums.length; i++){
           if(nums[i] != i) return i;
        }    
        return nums.length;
    }
}
```
* 注意：同样要谨慎改变输入。

3. 法三（哈希表）：扫描数组并存入`set`，再从$0$到$n$进行查找。
```java
class Solution {
    public int missingNumber(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for(int i : nums)
            set.add(i);
        for(int i = 0; i <= nums.length; i++)
            if(!set.contains(i))
                return i;
        return -1;
    }
}
```
* 注意：数组的遍历可采用下标的方式或者`foreach`方式。

4. 法四（数学法）：缺失元素就等于$\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$再减去数组元素总和。
```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length, numSum = 0;
        int sum = n*(n+1)/2;
        for(int i = 0; i< nums.length; i++)
            numSum += nums[i];
        return sum - numSum;
    }
}
```
* 注意：$n$太大时有可能溢出。

5. 法五（位运算法）：异或运算满足结合律，因此可设其初始值为$n$，随后与`nums[i]`和`i`做异或运算，便能得到缺失的元素。
```java
class Solution {
    public int missingNumber(int[] nums) {
        int ans = nums.length;
        for(int i = 0; i< nums.length; i++)
            ans ^= nums[i]^i;
        return ans;
    }
}
```