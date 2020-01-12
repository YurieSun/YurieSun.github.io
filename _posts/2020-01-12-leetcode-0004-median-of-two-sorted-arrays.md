---
layout: post
title: LeetCode 0004 题解
description: "寻找两个有序数组的中位数"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

### 题解
1. 法一（归并，不符合时间复杂度要求）：将两个有序数组合并到一个有序数中去，这是归并排序的一部分。时间复杂度为 $O(m+n)$ ，空间复杂度也为 $O(m+n)$ 。
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int k = 0, i = 0, j = 0;
        double[] nums = new double[m+n];
        while(k < m + n){
            if(i == m)
                nums[k++] = nums2[j++];
            else if(j == n)
                nums[k++] = nums1[i++];
            else if(nums1[i] < nums2[j])
                nums[k++] = nums1[i++];
            else
                nums[k++] = nums2[j++];
        }
        if((m + n) % 2 != 0)
            return nums[(m + n) / 2];
        else
            return (nums[(m + n) / 2 - 1] + nums[(m + n) / 2]) / 2;
    }
}
```
   * 改进：(1) 不使用额外数组存储合并后的元素。数组长度`len`为奇数时，需要找到第`len/2+1`个元素作为中位数，其数组下标应为`len/2`；数组长度为偶数时，需要找到第`len/2`和第`len+1`个元素计算中位数，其数组下标分别为`len/2-1`和`len/2`。因此，将这两种情况合并，采用两个变量`left`和`right`分别存储按顺序查找的上次元素和本次元素，找到第`len/2`个元素即可停止查看，最后根据奇、偶返回结果。(2)通过以上描述可知，只对所有元素的一半进行了访问，则对于用数组存储的方法来说，也可减少数组查看次数。时间复杂度为 $O(m+n)$ ，空间复杂度为 $O(1)$ 。
   ```java
    class Solution {
        public double findMedianSortedArrays(int[] nums1, int[] nums2) {
            int m = nums1.length, n = nums2.length;
            int i = 0, j = 0;
            int left = 0, right = 0;
            while(i + j <= (m + n) / 2){
                left = right;
                if(i == m)
                    right = nums2[j++];
                else if(j == n)
                    right = nums1[i++];
                else if(nums1[i] < nums2[j])
                    right = nums1[i++];
                else
                    right = nums2[j++];
            }
            if((m + n) % 2 != 0)
                return right;
            else
                return (left + right) / 2.0;
        }
    }
   ```
   * 更简洁的写法（将四种情况合并为两种，同时采用位运算代替模运算）：
   ```java
    class Solution {
        public double findMedianSortedArrays(int[] nums1, int[] nums2) {
            int m = nums1.length, n = nums2.length;
            int i = 0, j = 0;
            int left = 0, right = 0;
            while(i + j <= (m + n) / 2){
                left = right;
                if(j == n || (i < m && nums1[i] < nums2[j]))
                    right = nums1[i++];
                else
                    right = nums2[j++];
            }
            if(((m + n) & 1) != 0) // 通过查看一个数的最后一位是1还是0判断其奇偶性。
                return right;
            else
                return (left + right) / 2.0;
        }
    }
    ```
2. 法二（二分法）：通过`i`将`nums1`分成左右两部分，通过`j`将`nums2`分成左右两部分，使得若两数组元素总数为偶数时，分成的左、右两部分的元素个数相等，若两数组元素总数为奇数时，左边的元素个数比右边的元素个数多1，这样就可得到`i`与`j`的关系为 $i+j=(m+n+1)/2$。此时，若左半部分元素的最大值小于右半部分元素的最小值，那么就找到了中位数的位置：若`m+n`为奇数，则左半部分的最大值即为中位数；若`m+n`为奇数，则左半部分的最大值和右半部分的最小值的平均值即为中位数。则关键在于找到`i`的正确位置，而`j`的位置可以通过`i`固定下来，因此对`i`进行二分。若要满足左半部分元素的最大值小于右半部分元素的最小值，则需满足`nums1[i-1]<nums2[j]`及`nums2[j-1]<nums1[i]`，当不满足时，则移动左右边界，直至找到`i`的正确位置，再通过分类讨论得到中位数。计算过程中要注意一些边界条件的处理。
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int left = 0, right = m;
        // 保证m <= n，使得0 <= j <= n。
        if(m > n)
            return findMedianSortedArrays(nums2, nums1);
        while(left <= right){
            int i = (left + right) / 2;
            int j = (m + n + 1) / 2 - i;
            // i太大。
            if(i > left && nums1[i - 1] > nums2[j])
                right = i - 1;
            // i太小。
            else if(i < right && nums2[j - 1] > nums1[i])
                left = i + 1;
            // 找到符合的i。
            else{
                // 找到左边最大值，并处理边界条件：当左半部分都是某一个数组的元素时。
                int maxLeft = 0;
                if(i == 0)
                    maxLeft = nums2[j - 1];
                else if(j == 0)
                    maxLeft = nums1[i - 1];
                else
                    maxLeft = Math.max(nums1[i - 1], nums2[j - 1]);
                // 奇数直接返回左边最大值
                if(((m + n) & 1) != 0)
                    return maxLeft;
                //找到右边最大值，并处理边界条件：当右半部分都是某一个数组的元素时。
                int minRight = 0;
                if(i == m)
                    minRight = nums2[j];
                else if(j == n)
                    minRight = nums1[i];
                else
                    minRight = Math.min(nums1[i], nums2[j]);
                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0;
    }
}
```
3. 法三（递归）：转化为求第$k$小的数 。时间复杂度为 $O(\log(m+n))$ ，空间复杂度是$O(1)$（由于是尾递归）。
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int left = (m + n + 1) / 2, right = (m + n + 2) / 2;
        // 如果是奇数，left和right相等，相当于对中位数算了两遍又除以2；
        // 如果是偶数，则得到了中间的两个数，并求平均。
        return (getKth(nums1, 0, m - 1, nums2, 0, n - 1, left) + getKth(nums1, 0, m - 1, nums2, 0, n - 1, right)) / 2.0;
    }
    public int getKth(int[] nums1, int start1, int end1, int[] nums2, int start2, int end2, int k){
        int len1 = end1 - start1 + 1, len2 = end2 - start2 + 1;
        // 保证nums1的长度小于nums2，这样若有数组空了，一定是nums1。
        if(len1 > len2) 
            return getKth(nums2, start2, end2, nums1, start1, end1, k);
        // nums1为空时，直接返回nums2的第k个元素。
        if(len1 == 0) 
            return nums2[start2 + k - 1];
        if(k == 1)
            return Math.min(nums1[start1], nums2[start2]);
        // 分别取两个数组的第k/2个元素，若超出数组长度，则取数组末尾元素。
        int i = start1 + Math.min(len1, k / 2) - 1;
        int j = start2 + Math.min(len2, k / 2) - 1;
        // 在这种情况下，nums2的前j个元素一定不会是第k小的元素，则将这些元素舍去，同时减小k。
        if(nums1[i] > nums2[j])
            return getKth(nums1, start1, end1, nums2, j + 1, end2, k - (j - start2 + 1));
        else
            return getKth(nums1, i + 1, end1, nums2, start2, end2, k - (i - start1 + 1));
    }
}
```