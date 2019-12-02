---
layout: post
title: LeetCode 0717 题解
description: "1比特与2比特字符"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[1比特与2比特字符](https://leetcode-cn.com/problems/1-bit-and-2-bit-characters/)

### 题解
1. 法一：扫描数组，若元素为1，则不管是10还是11，均为2比特字符；若为0，则只能是1比特字符；扫描结束后，检验这个指针是否指向数组最后一项，若是，则表明该位置上的0是1比特字符，反之则不是。
```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int i = 0;
        while(i < bits.length-1){
            if(bits[i] == 0)
                i++;
            else
                i += 2;
        }
        return i == bits.length-1;
    }
}
```
* 利用`bits[i]`等于0或1的特点，可以更简洁地写为：
```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int i = 0;
        while(i < bits.length-1)
            i += bits[i] + 1;
        return i == bits.length-1;
    }
}
```
2. 法二：仅根据最后一个0和倒数第二个0之间的1的个数，即可进行判断。从后往前扫描数组，若1的个数为偶数，则正好两两配对，0为1比特字符；若为奇数，则还剩下一个1要与0配对，即0为2比特字符。
```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int count = 0;
        for(int i = bits.length-2; i >= 0; i--){
            if(bits[i] == 1)
                count++;
            else
                break;
        }
        return count % 2 == 0;
    }
}
```