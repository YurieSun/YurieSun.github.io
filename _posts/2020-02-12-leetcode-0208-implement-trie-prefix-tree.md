---
layout: post
title: LeetCode 0208 题解
description: "实现 Trie (前缀树)"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

### 题解
1. 法一

```java
class TrieNode{
    TrieNode[] child;
    boolean isEnd;
    public TrieNode(){
        child = new TrieNode[26];
        isEnd = false;
    }
}

class Trie {
    TrieNode root;
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode pre = root;
        for(int i = 0; i < word.length(); i++){
            char cur = word.charAt(i);
            if(pre.child[cur - 'a'] == null)
                pre.child[cur - 'a'] = new TrieNode();
            pre = pre.child[cur - 'a'];
        }
        pre.isEnd = true;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode pre = root;
        for(int i = 0; i < word.length(); i++){
            char cur = word.charAt(i);
            if(pre.child[cur - 'a'] == null)
                return false;
            pre = pre.child[cur - 'a'];
        }
        return pre.isEnd;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode pre = root;
        for(int i = 0; i < prefix.length(); i++){
            char cur = prefix.charAt(i);
            if(pre.child[cur - 'a'] == null)
                return false;
            pre = pre.child[cur - 'a'];
        }
        return true;
    }
}
/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```