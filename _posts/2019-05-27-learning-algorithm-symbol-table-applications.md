---
layout: post
title: 算法笔记（十三）
description: "学习资料：algorithm (Princeton University)"
keywords: test
category: Learning
tags: [algorithm]
---

## Symbol Table Applications
### Sets
1. Set API
    ```java
    // set是指一个含有不同键的集合
    public class SET<Key extends Comparable<Key>>
        SET() 
        void add(Key key) 
        boolean contains(Key key)
        void remove(Key key)
        int size()
        Iterator<Key> iterator()
    ```
2. Exception filter
    * 操作  
    读入一个单词列表，将标准输入中在/不在这个列表上的单词打印出来，即白名单（黑名单）。
    * 应用

    |application|purpose|key|in list|
    |:-:|:-:|:-:|:-:|
    |spell checker|identify misspelled words|word|dictionary words|
    |browser|mark visited pages|URL|visited pages|
    |parental comtrols|block sites|URL|bad sites|
    |chess|detect draw|board|positions|
    |spam filter|eliminate spam|IP address|spam addresses|
    |credit cards|check for stolen cards|number|stolen cards|
3. java实现
    ```java
    //白名单
    public class Whitelist
    {
        SET<String> set = new SET<String>();
        //读入白名单
        In in = new In(args[0]);
        while (!in.isEmpty)
            set.add(in.readString());
        //打印白名单中含有的字符
        while (!StdIn.isEmpty())
        {
            String word = StdIn.readString();
            if (set.contains(word))
                StdOut.println(word);
        }
    }
    //黑名单
    public class Blacklist
    {
        SET<String> set = new SET<String>();
        //读入黑名单
        In in = new In(args[0]);
        while (!in.isEmpty)
            set.add(in.readString());
        //打印除黑名单中之外的字符
        while (!StdIn.isEmpty())
        {
            String word = StdIn.readString();
            if (!set.contains(word))
                StdOut.println(word);
        }
    }
    ```

### Dictionary Clients
1. 字典查询  
    * 目标
    给定key查询对应的value
    * 参数  
    一个以逗号分隔的value文件；key字段；value字段
2. java实现  
    ```java
    public class LookupCSV
    {
        public static void main(String[] args)
        {
            //处理输入文件
            In in = new In(args[0]);
            int keyField = Integer.parseInt(args[1]);
            int valField = Integer.parseInt(args[2]);
            //创建ST
            ST<String, String> st = new ST<String, String>();
            while (!in.isEmpty())
            {
                String line = in.readLine();
                String[] tokens = line.split(",");
                String key = tokens[keyField];
                String val = tokens[valField];
                st.put(key, val);
            }
            //处理标准输入的查询
            while (!Std.isEmpty())
            {
                String s = StdIn.readString();
                if (!st.contains(s)) StdOut.println("Not found");
                else StdOut.println(st.get(s));
            }
        }
    }
    ```

### Indexing Clients
1. file indexing
    * 目标  
    给定文件列表，创建索引，以便快速找到给定查询字符串的所有文件。
    * 方法  
    key = query string; value = set of files containing that string.
    * java实现  
    ```java
    import java.io.File;
    public class FileIndex
    {
        public static void main(String[] args)
        {
            //创建ST
            ST<String, SET<File>> st = new ST<String, SET<File>>();
            //对于输入的每个单词，将文件名放到对应的set中。
            for (String filename : args)
            {
                File file = new File(filename);
                In in = new In(file);
                while (!in.isEmpty())
                {
                    String key = in.readString();
                    if (!st.contains(key))
                        st.put(word, new SET<File>());
                    SET<File> set = st.get(key);
                    set.add(file);
                }
            }
            //查询
            while (!StdIn.isEmpty())
            {
                String query = StdIn.readString();
                StdOut.println(st.get(query));
            }
        }
    }
    ```
2. concordance
    * 目标  
    对文集做预处理以支持一致性查询：即对于给定的单词，找到所有含有该单词的上下文
    * java实现
    ```java
    public class Concordance
    {
        public static void main(String[] args)
        {
            In in = new In(args[0]);
            String words = StdIn.readAll().split("\\s+");
            ST<String, SET<Integer>> st = new ST<String, SET<Integer>>();
            for (int i = 0; i < words.length; i++)
            {
                String s = words[i];
                if (!st.contains(s))
                    st.put(s, new SET<Integer>());
                SET<Integer> set = st.get(s);
                set.add(i);
            }
            while (!StdIn.isEmpty())
            {
                String query = StdIn.readString();
                SET<Integer> set = st.get(query);
                for (int k : set)
                    //print words[k-4] to words[k+4]
            }
        }
    }
    ```

### Sparse Vectors