---
layout: post
title: LeetCode 0164 题解
description: "最大间距"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最大间距](https://leetcode-cn.com/problems/maximum-gap/)

### 题解
1. 法一（排序）：对数组排序后，得到相邻两元素间的最大差值。
```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums.length < 2)
            return 0;
        Arrays.sort(nums);
        int max = 0;
        for(int i = 1; i < nums.length; i++)
            max = Math.max(max, nums[i] - nums[i - 1]);
        return max;
    }
}
```
   * 时间复杂度为 $O(n\lg n)$。
2. 法二（基数排序）：先按照个位数进行排序，再按十位数进行排序，依次向上，就能将数组变为有序。
```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums.length < 2)
            return 0;
        int max = Integer.MIN_VALUE;
        for(int i : nums)
            max = Math.max(max, i);
        // 用list存放每次排序后的元素
        List<ArrayList<Integer>> list = new ArrayList<>(); 
        for(int i = 0; i < 10; i++)
            list.add(new ArrayList<>());
        int exp = 1;
        while(max > 0){
            // 每次排序前先将list中的元素清空
            for(int i = 0; i < 10; i++)
                list.set(i, new ArrayList<>());
            // 将元素进行排序并放入list中
            for(int i : nums)
                list.get(i / exp % 10).add(i);
            int index = 0;
            // 将排序后的元素放回数组中
            for(int i = 0; i < list.size(); i++)
                for(int j = 0; j < list.get(i).size(); j++)
                    nums[index++] = list.get(i).get(j); 
            exp *= 10;
            max /= 10;
        }
        int maxGap = 0;
        for(int i = 1; i < nums.length; i++)
            maxGap = Math.max(maxGap, nums[i] - nums[i - 1]);
        return maxGap;
    }
}
```
   * 时间复杂度为 $O(d \cdot (n+k))$。基数排序相当于$d$次计数排序，而计数排序的时间复杂度为$O(n+k)$，其中$k$为数组元素的基数。本题中，将元素分成10个组进行存储，因此 $k=10$；而`int`的最大值为`2147483647`，其位数等于10，则最多进行10次计数排序，即 $d \le 10$ 为常数。综上，以上算法的时间复杂度为 $O(d \cdot (n+k)) \approx O(n)$。
3. 法三（桶排序 + 抽屉原理）：对于桶长度为`bucketSize = (max - min) / (n - 1))`的情况，每个桶中元素的最大间距即为`bucketSize`。若元素均匀排列，则在桶数量为`int bucketNum = (max - min) / bucketSize + 1`时，每个桶中都有一个元素，那么不均匀排列就会出现一个空桶，则相邻的跨桶元素之间的间距会大于桶中最大间距`bucketSize`。因此，在设置好桶长度与桶数量后，找到当前桶的最小值与前一个桶的最大值之间的最大差值即为元素排序后的最大间距。
```java
class Solution {
    public int maximumGap(int[] nums) {
        int n = nums.length;
        if(n < 2)
            return 0;
        int max = Integer.MIN_VALUE, min = Integer.MAX_VALUE;
        // 找到数组中的最大值和最小值
        for(int i ： nums){
            max = Math.max(max, i);
            min = Math.min(min, i);
        }
        if(max == min)
            return 0;
        // 桶长度：这里要避免(max - min)小于(n - 1)从而使得下面的被除数为0的情况；
        // 在这种情况下，将桶长度设为1。
        int bucketSize = Math.max(1, (max - min) / (n - 1));
        // 桶数量：n个数之间有n-1个间隔，但在此基础上还再多加1个桶；
        // 这是因为按照这种划分方法，每个桶区间时左闭右开的，为了存放最右边的数就需要为其多加1个桶。
        int bucketNum = (max - min) / bucketSize + 1;
        // 为每个桶区间的最大值、最小值分别开辟区间。
        int[] bucketMax = new int[bucketNum];
        int[] bucketMin = new int[bucketNum];
        Arrays.fill(bucketMax, Integer.MIN_VALUE);
        Arrays.fill(bucketMin, Integer.MAX_VALUE);
        // 将元素放入各自的桶中。
        for(int i = 0; i < nums.length; i++){
            int index = (nums[i] - min) / bucketSize;
            bucketMax[index] = Math.max(bucketMax[index], nums[i]);
            bucketMin[index] = Math.min(bucketMin[index], nums[i]);
        }
        int preMax = Integer.MIN_VALUE, maxGap = 0;
        // 遍历每个桶，用当前桶的最小值减去前一个桶的最大值得到可能的最大间距。
        for(int i = 0; i < bucketNum; i++){
            if(preMax != Integer.MIN_VALUE && bucketMin[i] != Integer.MAX_VALUE)
                maxGap = Math.max(maxGap, bucketMin[i] - preMax);
            if(bucketMax[i] != Integer.MIN_VALUE)
                preMax = bucketMax[i];
        }
        return maxGap;
    }
}
```
   * 时间复杂度为 $O(n)$。