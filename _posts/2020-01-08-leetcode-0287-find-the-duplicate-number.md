---
layout: post
title: LeetCode 0287 题解
description: "寻找重复数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

### 题解
1. 法一：比较排序、用Set集合保存元素、桶排序均不满足题目的要求。
2. 法二（快慢指针检测循环）：若存在重复元素，则将数组看成链表将会出现环状结构，而重复元素正好是环的起始元素，因此需找到环的起始位置。快指针`fast`每次走两步，慢指针`slow`每次走一步，直至相遇；再设置一个指针`finder`从数组起始位置开始走，每次走一步，慢指针从上次位置继续走，同样每次走一步，两指针相遇时即到达环的起始位置。
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int fast = 0, slow = 0;
        // 快指针与慢指针
        do{
            fast = nums[nums[fast]];
            slow = nums[slow];
        }while(fast != slow);
        int finder = 0;
        // 慢指针与finder指针
        while(finder != slow){
            finder = nums[finder];
            slow = nums[slow];
        }
        return finder;
    }
}
```
   * 时间复杂度为 $O(n)$。
3. 法三（二分查找）：所有元素均在`1`到`n`之间，设其中位数为`mid`，若无重复元素，则`1`到`mid-1`的元素个数应小于`mid`。那么当存在重复元素时，若`1`到`mid-1`的元素个数小于`mid`，说明重复元素在后半段即`mid`到`n`中；若`1`到`mid-1`的元素个数应大于等于`mid`，则说明重复元素在前半段`1`到`mid-1`中。依次二分，当只剩一个元素时，该元素就是重复元素。
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int left = 1, right = nums.length - 1;
        while(left < right){
            int mid = left + (right - left + 1) / 2;
            int count = 0;
            for(int n : nums){
                if(n < mid)
                    count++;
            }
            if(count < mid)
                left = mid;
            else
                right = mid - 1;
        }
        return left;
    }
}
```
   * 时间复杂度为 $O(n \lg n)$。
* 附桶+抽屉原理的算法
```java
class Solution {
    public int findDuplicate(int[] nums) {
        for(int i = 0; i < nums.length; i++){
            // 注意：只能先将nums[i]先放到它应该在的位置上，不能先把nums[nums[i] - 1]放到nums[i]上，
            // 否则会导致nums[i]发生改变，使得后续计算nums[nums[i] - 1]出错。
            while(nums[i] != nums[nums[i] - 1]){
                int temp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
            }
        }
        for(int i = 0; i < nums.length; i++){
            if(nums[i] != i + 1)
                return nums[i];
        }
        return 0;
    }
}
```