---
layout: post
title: LeetCode 0299 题解
description: "猜数字游戏"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[猜数字游戏](https://leetcode-cn.com/problems/bulls-and-cows/)

### 题解
1. 法一：两次遍历。第一次遍历guess数组，若元素匹配，则`bull`加1；否则将`secret`和`guess`的元素分别放入哈希表和数组中，其中哈希表记录`secret`中不匹配元素及其出现次数，数组记录`guess`中不匹配的元素。第二次遍历存放不匹配元素的数组，若在哈希表中有该元素，则`cow`加1，同时将哈希表中的次数减1，若次数达到0，则将该元素从哈希表中删去。
```java
class Solution {
    public String getHint(String secret, String guess) {
        int bulls = 0, cows = 0;
        Map<Character, Integer> map = new HashMap<>();
        List<Character> miss = new ArrayList<>();
        for(int i = 0; i< guess.length(); i++){
            if(guess.charAt(i) == secret.charAt(i))
                bulls++;
            else{
                map.put(secret.charAt(i), map.getOrDefault(secret.charAt(i), 0)+1);
                miss.add(guess.charAt(i));
            }
        } 
        for(char x : miss){
            if(map.containsKey(x)){
                cows++;
                if(map.get(x)-1 != 0)
                    map.put(x, map.get(x)-1);
                else
                    map.remove(x);
            }
        }
        return bulls + "A" + cows +"B";
    }
}
```
2. 法二：由于包含的数字是$1$到$9$，因此可用两个数组分别记录`secret`和`guess`各自的不匹配元素的出现次数，而对于每个不匹配的元素，较小的出现次数即为才对该元素的个数，因此将所有元素的较小出现次数相加，即为`cow`。（其实思路和法一相同，只是数据存储方式不同。）
```java
class Solution {
    public String getHint(String secret, String guess) {
        int bulls = 0, cows = 0;
        int[] sec = new int[10];
        int[] gue = new int[10];
        for(int i = 0; i < secret.length(); i++){
            if(secret.charAt(i) == guess.charAt(i))
                bulls++;
            else{
                sec[secret.charAt(i) - '0']++;
                gue[guess.charAt(i) - '0']++;
            }
        }
        for(int i = 0; i < 10; i++)
            cows += Math.min(sec[i], gue[i]);
        return bulls + "A" + cows + "B";
    }
}
```
* 注意：使用数组（法二）比使用哈希表（法一）的速度要快。
3. 法三：一次遍历数组，对`bulls`和`cows`计数。（非常巧妙）
```java
public String getHint(String secret, String guess) {
    int bulls = 0;
    int cows = 0;
    int[] numbers = new int[10];
    for (int i = 0; i<secret.length(); i++) {
        int s = Character.getNumericValue(secret.charAt(i));
        int g = Character.getNumericValue(guess.charAt(i));
        if (s == g) bulls++;
        else {
            // numbers[s] < 0表示secret的当前元素之前在guess中出现了。
            if (numbers[s] < 0) cows++;
            // numbers[g] > 0表示guess的当前元素之前在secret中出现了。
            if (numbers[g] > 0) cows++;
            numbers[s] ++;
            numbers[g] --;
        }
    }
    return bulls + "A" + cows + "B";
}
```
* 更简洁的写法：
```java
public String getHint(String secret, String guess) {
    int bulls = 0;
    int cows = 0;
    int[] numbers = new int[10];
    for (int i = 0; i<secret.length(); i++) {
        if (secret.charAt(i) == guess.charAt(i)) bulls++;
        else {
            if (numbers[secret.charAt(i)-'0']++ < 0) cows++;
            if (numbers[guess.charAt(i)-'0']-- > 0) cows++;
        }
    }
    return bulls + "A" + cows + "B";
}
```