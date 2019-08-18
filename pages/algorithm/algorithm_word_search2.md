---
title: "Word Search II"
tags: [algorithm, trie]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_word_search2.html
folder: algorithm
toc: false
---

## Description: Hard
Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

Note:
* All inputs are consist of lowercase letters a-z.
* The values of words are distinct.

### Example
* Input: words = ["oath","pea","eat","rain"], matrix 如下：
  ```
  ['o','a','a','n']
  ['e','t','a','e']
  ['i','h','k','r']
  ['i','f','l','v']
  ```
  * Output: Output: ["eat","oath"]

## Solution: 我自己的 Trie + DFS 的实现。速度 前50%。将来有空可以看下如何进一步提速

### Complexity
* Time: O(n * m * len^4)，len是dictionary里的words的平均长度 <==== 对么？？？？
* Space: O(n * m) <==== ？？？？

### Java
对于Trie，我有专门的笔记整理过，在这个repo里。
```java
public class Solution {
    private static final int[][] DIRS = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1}
    };
    
    public List<String> findWords(char[][] matrix, String[] words) {
        Set<String> dict = new HashSet<>();
        for (String w : words) dict.add(w);
        
        Set<String> tmpResult = new HashSet<>(); // 在matrix里找到的stings要去重！
        
        Trie trie = new Trie();
        for (String word : dict) {
            trie.insertWord(word);
        }
        
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                StringBuilder sb = new StringBuilder();
                // visited矩阵会比一个set of int array 或者 set of string 快很多！
                // 有时候最简单原始的才是最快的，高级货未必好
                boolean[][] visited = new boolean[matrix.length][matrix[0].length];

                getWords(matrix, i, j, trie.root, sb, visited, tmpResult);
            }
        }
        
        List<String> result = new LinkedList<>();
        for (String s : tmpResult) result.add(s);
        return result;
    }

    private void getWords(char[][] matrix, int i, int j, TrieNode node, 
            StringBuilder sb, boolean[][] visited, Set<String> tmpResult) {
        char curChar = matrix[i][j];
        TrieNode child = node.children[curChar - 'a'];
        
        if (child == null) {
            return;
        }

        sb.append(curChar);
        visited[i][j] = true;
        
        if (child.endOfWord) {
            tmpResult.add(sb.toString());
        }
        // 这里不要return！因为dictionary里面可能同时含有比如说 cat 和 category
        
        for (int[] dir : DIRS) {
            int newX = i + dir[0];
            int newY = j + dir[1];
            
            if (outOfBound(matrix, newX, newY)) continue;
            if (visited[newX][newY]) continue;

            getWords(matrix, newX, newY, child, sb, visited, tmpResult);
        }
        
        // 复原
        sb.deleteCharAt(sb.length() - 1);
        visited[i][j] = false;
    }
    
    private boolean outOfBound(char[][] matrix, int i, int j) {
        return i < 0 || i >= matrix.length || j < 0 || j >= matrix[0].length;
    }
}

// helper classes
class TrieNode {
    boolean endOfWord;
    TrieNode[] children;
    
    public TrieNode() {
        this.endOfWord = false;
        this.children = new TrieNode[26]; // 'a' to 'z'
    }
}

// Trie可以有很多member function，这里只放了一个insert word，因为本题只用到这个，
// 我把 given a trie node, check the next available chars 这个function 放到 solution class 里去了
class Trie { 
    TrieNode root;
    
    public Trie() {
        this.root = new TrieNode(); // 虚root，它不含任何char
    }
    
    public void insertWord(String word) {
        insertWord(word, 0, root);
    }
    
    public void insertWord(String word, int index, TrieNode node) {
        char curChar = word.charAt(index);
        TrieNode nextNode = node.children[curChar - 'a'];
        if (nextNode == null) {
            nextNode = new TrieNode();
            node.children[curChar - 'a'] = nextNode; // 这个不能少！
        }
        
        if (index == word.length() - 1) {
            nextNode.endOfWord = true;
            return;
        }
        
        insertWord(word, index + 1, nextNode);
    }
}
```

## Reference
* [Word Search II [LeetCode]](https://leetcode.com/problems/word-search-ii/description/)

{% include links.html %}
