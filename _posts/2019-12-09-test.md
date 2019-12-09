---
layout: post
title: LeetCode 0080 题解
description: "删除排序数组中的重复项 II"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[删除排序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

### 题解
1. 法一：遍历数组，先统计元素出现次数，若小于等于2，则进行双指针交换。（与[LeetCode 0026](https://yuriesun.github.io/leetcode/leetcode-0026-remove-duplicates-from-sorted-array)相比，在交换前多了元素次数的计算与判断。）
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int j = 0, count = 1;
        for(int i = 1; i < nums.length; i++){
            if(nums[i-1] != nums[i])
                count = 1;
            else
                count++;
            if(count <= 2){
                j++;
                nums[j] = nums[i];
                // 或者将这两句合并为nums[++j] = nums[i];
            }
        }
        return j+1;
    }
}
```
2. 法二：`i`为慢指针，指向交换元素后的位置；而当前元素`n`为快指针，若当前元素比`i`前面的两个位置大，说明该元素是新元素，并且可保证新元素最多出现两次。（由于题目中给定的是排序数组，则`if`条件中满足`n > nums[i-2]`；若数组未排序，且前面出现过的元素后面不会再出现，那么需将其改为`n != nums[i-2]`。如果给定数组既未排序，且未说明前面出现过的元素后面不会再出现，那么将数组排序后再用双指针即可。）
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0;
        for(int n : nums)
            if(i < 2 || n > nums[i-2])
                nums[i++] = n;
        return i;
    }
}
```
3. 法三：遍历字符串，记录相同字符的起始下标与结束下标。这里要注意的是循环变量`i`的结束值被设为`S.length()`，同样是为了判断整个字符串均为同一字母的情况。
```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String S) {
        List<List<Integer>> list = new ArrayList<>();
        int start = 0, end = 0;
        for(int i = 1; i <= S.length(); i++){
            if(i == S.length() || S.charAt(i-1) != S.charAt(i)) {
            // i == S.length()这个判断条件应放在前面，防止下标溢出
                if(end - start >= 2)
                    list.add(Arrays.asList(new Integer[]{start, end}));
                start = i;   
            }
            end = i;
        }
        return list;
    }
}
```
### 思考
1. 这两种方法均可拓展为【删除重复项且最多保留$k$个】的解法：法一中将`count <= 2`改为`count <= k`，法二中将`i < 2 || n > nums[i-2]`改为`i < k || n > nums[i-k]`即可。
2. 进一步，[26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)也可采用这种做法，代码非常简洁。
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0;
        for(int n : nums)
            if(i < 1 || n > nums[i-1])
                nums[i++] = n;
        return i;
    }
}
```
* 或者
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0;
        for(int n : nums)
            if(n != nums[i])
                nums[++i] = n;
        return i+1;
    }
}
```
3. 同理，[27. 移除元素](https://leetcode-cn.com/problems/remove-element/)使用该方法的代码如下:
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int i = 0;
        for(int n :nums)
            if(n != val)
                nums[i++] = n;
        return i;
    }
}
```
* 对这题所写的题解[Java：通过foreach巧妙实现双指针](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/solution/javatong-guo-foreachqiao-miao-shi-xian-shuang-zhi-/)。
其具体内容与上述一致。