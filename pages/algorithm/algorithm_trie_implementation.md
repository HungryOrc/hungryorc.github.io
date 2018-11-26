---
title: "Trie Implementation"
tags: [algorithm, algorithm_overview]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_trie_implementation.html
folder: algorithm
toc: false
---

## Overview
本 Trie（每个 Node 里存的 value 是一个 char）要实现的操作：
* 在每个 Trie Node 上存一个 int size，记录从 Trie root 开始到本node为止组成的这个prefix，后面一共有多少个word
  * 如果到本 node 为止，恰好形成一个完整的word，那么这个word也要算到这个 size 里去
* 往Trie里插入一个 String
* 查找一个 String 是否在Trie里作为一个完整的 word
* 查找一个 Prefix String 是否存在于Trie里，且返回其最后一个node，如果false，则返回null
* 返回一个 Prefix String 在Trie里的所有 后继(Subsequent) Strings，用 List<String> 来表示
   
### Complexity
* Time:
  * 找一个String是否存在：O(这个String的长度)
  * 找某个字母（这个字母可能在某个String里的某个位置）后面有没有接另外一个字母：O(1)
* Space: O(所有Strings的长度和)

## Implementation 1: Node的Children用`Map<Character, Node>`表示，Trie的member methods用Recursion实现

下面实现iteration！！！并申明不是一一绑定的！！！！！！！

### Java
```java
class TrieNode {
    char val;
    Map<Character, TrieNode> children;
    boolean endOfWord; // end of word以后，后面还可能继续有别的word！例：Trie里同时存 car 和 card 的情况
    int size; // // number of all descendants, including this node itself if it is an end of a word
    
    public TrieNode(char c) {
        this.val = c;
        this.children = new HashMap<>();
    }
}

public class Trie {
    Node root;
    
    public Trie() {
        root = new Node('');
    }
    
    // insert a word into the Trie
    // ----------------------------------------------------------------
    public void insert(String word) {
        insert(root, word, 0);
        root.size ++;
    }
    
    private void insert(Node node, String word, int index) {
        if (index == word.length()) {
            return;
        }
        
        char c = word.charAt(index);
        Node child = node.children.get(c);
        
        if (child == null) {
            child = new Node(c);
            node.children.put(c, child);
        }
        
        child.size++;
        
        if (index == word.length() - 1) {
            child.endOfWord = true;
        } else {
            insert(child, word, index + 1);
        }
    }
    
    // Search for a word in the Trie
    // ----------------------------------------------------------------
    public boolean search(String word) {
        return search(root, word, 0);
    }
    
    private boolean search(Node node, String word, int index) {
        char c = word.charAt(index);
        Node child = node.children.get(c);
        
        if (child == null) {
            return false;
        }
        
        if (index == word.length() - 1) {
            if (child.endOfWord == true) {
                return true;
            }
            return false;
        }
        
        return search(child, word, index + 1);
    }
    
    // Delete a word in the trie
    // 方法：首先在trie里找这个word，
    // 如果这个word根本无法在trie里完整地找到，或者完整找到了，但word的最后一个char不是endOfWord，都算没找到。
    // 如果找到了，就把这个word的每个char node的size--，把最后一个char node的endOfWord设为false。
    // 如果被删除word的最后一个char node的size为0了，即它后面再没有任何children，那么可以把这个node设为null，也可以不管它
    // ----------------------------------------------------------------
    public boolean delete(String word) {
        if (!this.search(word)) {
            return false;
        }
    
        return delete(root, word, 0);
    }
    
    private boolean delete(Node node, String word, int index) {
        char c = word.charAt(index);
        Node child = node.children.get(c);
        
        if (child == null) {
            return false;
        }
        
        child.size--;
        
        if (index == word.length() - 1) {
            if (child.endOfWord == true) {
                child.endOfWord = false;
                
                if (child.size == 0) {
                    node.children.remove(c);
                }
            
                return true;
            }
            return false;
        }
        
        return delete(child, word, index + 1);
    }   
   
}


  
  
```

## Reference
* [Trie | Insert and Search [GeeksforGeeks]](https://www.geeksforgeeks.org/trie-insert-and-search/)
* [Trie | Delete [GeeksforGeeks]](https://www.geeksforgeeks.org/trie-delete/)

{% include links.html %}
