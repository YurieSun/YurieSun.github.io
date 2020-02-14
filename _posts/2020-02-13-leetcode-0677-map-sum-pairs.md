---
layout: post
title: LeetCode 0677 题解
description: "键值映射"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[键值映射](https://leetcode-cn.com/problems/map-sum-pairs/)

### 题解
1. 法一（Trie + 哈希表）：哈希表用于存储插入的键值对，前缀树用来存储字符中每个前缀及其对应的值。在更新前缀树中的值时，先在哈希表中查找之前是否存过该键，如果有，就得到需要增加的值`inc`，以更新遍历过程中的所有值。

```java
class Node{
    Node[] child;
    int sum;
    public Node(){
        child = new Node[26];
    }
}

class MapSum {
    // Map<Node, Integer> map;
    Node root;
    Map<String, Integer> map;
    /** Initialize your data structure here. */
    public MapSum() {
        // map = new HashMap<Node, Integer>();
        root = new Node();
        map = new HashMap<>();
    }
    
    public void insert(String key, int val) {
        Node pre = root;
        int inc = val;
        if(map.containsKey(key))
            inc -= map.get(key);
        for(int i = 0; i < key.length(); i++){
            char cur = key.charAt(i);
            if(pre.child[cur - 'a'] == null){
                pre.child[cur - 'a'] = new Node();  
            }        
            pre = pre.child[cur - 'a'];  
            pre.sum += inc;        
        }
        map.put(key, val);
    }
    
    public int sum(String prefix) {
        Node pre = root;
        for(int i = 0; i < prefix.length(); i++){
            char cur = prefix.charAt(i);
            if(pre.child[cur - 'a'] == null)
                return 0;
            pre = pre.child[cur - 'a'];
        }
        return pre.sum;
    }
}

/**
 * Your MapSum object will be instantiated and called as such:
 * MapSum obj = new MapSum();
 * obj.insert(key,val);
 * int param_2 = obj.sum(prefix);
 */
```
* 上述方法是采用了哈希表来记录每个单词的键值对，以查找之前是否存在该单词。也可以通过已经建立的前缀树来查找，这样会降低空间复杂度，增加时间复杂度。

```java
class Node{
    Node[] child;
    boolean isEnd;
    int sum;
    public Node(){
        child = new Node[26];
    }
}

class MapSum {
    Node root;
    /** Initialize your data structure here. */
    public MapSum() {
        root = new Node();
    }
    
    public void insert(String key, int val) {
        Node pre = root;
        int inc = val;
        int res = find(key);
        inc -= res;
        for(int i = 0; i < key.length(); i++){
            char cur = key.charAt(i);
            if(pre.child[cur - 'a'] == null){
                pre.child[cur - 'a'] = new Node();  
            }        
            pre = pre.child[cur - 'a'];  
            pre.sum += inc;        
        }
        pre.isEnd = true;
    }
    
    public int sum(String prefix) {
        Node pre = root;
        for(int i = 0; i < prefix.length(); i++){
            char cur = prefix.charAt(i);
            if(pre.child[cur - 'a'] == null)
                return 0;
            pre = pre.child[cur - 'a'];
        }
        return pre.sum;
    }

    public int find(String word){
        Node pre = root;
        for(int i = 0; i < word.length(); i++){
            char cur = word.charAt(i);
            if(pre.child[cur - 'a'] == null)
                return 0;
            pre = pre.child[cur - 'a'];
        }
        return pre.isEnd ? pre.sum : 0;
    }
}
```
2. 法二（两个哈希表）：也可以使用一个哈希表来代替Trie存储字符中每个前缀及其对应的值。

```java
class MapSum {
    // Map<Node, Integer> map;
    Map<String, Integer> map;
    Map<String, Integer> trie;
    /** Initialize your data structure here. */
    public MapSum() {
        // map = new HashMap<Node, Integer>();
        map = new HashMap<>();
        trie = new HashMap<>();
    }
    
    public void insert(String key, int val) {
        int inc = val;
        if(map.containsKey(key))
            inc -= map.get(key);
        for(int i = 1; i <= key.length(); i++){
            String cur = key.substring(0, i);
            trie.put(cur, trie.getOrDefault(cur, 0) + inc);        
        }
        map.put(key, val);
    }
    
    public int sum(String prefix) {
        if(trie.containsKey(prefix))
            return trie.get(prefix);
        return 0;
    }
}
```