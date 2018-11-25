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

## Implementation 1: 每个 Node 的 Children 用 `Map<Character, Node>` 表示

### Java
```java
// Trie Node class
class Node {
    char val;
    Map<Character, Node> children;
    boolean endOfWord; // end of word以后，后面还可能继续有别的word！例：Trie里同时存 car 和 card 的情况
    int size; // // number of all descendants, including this node itself if it is an end of a word
    
    public Node(char c) {
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
}


    def _deleteHelper(self,pNode,key,level,length): 
        ''' 
        Helper function for deleting key from trie 
        '''
        if pNode: 
            # Base case 
            if level == length: 
                if pNode.value: 
                    # unmark leaf node 
                    pNode.value = 0
  
                # if empty, node to be deleted 
                return pNode.isItFreeNode() 
  
            # recursive case 
            else: 
                index = self._Index(key[level]) 
                if self._deleteHelper(pNode.children[index],\ 
                                        key,level+1,length): 
  
                    # last node marked,delete it 
                    del pNode.children[index] 
  
                    # recursively climb up and delete  
                    # eligible nodes 
                    return (not pNode.leafNode() and \ 
                                pNode.isItFreeNode()) 
  
        return False
  
    def deleteKey(self,key): 
        ''' 
        Delete key from trie 
        '''
        length = len(key) 
        if length > 0: 
            self._deleteHelper(self.root,key,0,length)     
  
  
  
```

## Reference
* [Trie | (Insert and Search) [GeeksforGeeks]](https://www.geeksforgeeks.org/trie-insert-and-search/)

{% include links.html %}
