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
### 这个 Trie 要实现的操作
* 每个 TrieNode 里存的 value 是一个 char，从`a`到·`z`的小写字母，共26种可能的值
* 在每个 TrieNode 上存一个 int size，记录从 Trie root 开始到本node为止组成的这个prefix，后面一共有多少个word
  * 如果到本 Node 为止，恰好形成一个完整的word，那么这个word也要算到这个 size 里去
* 往Trie里插入一个 String
* 查找一个 String 是否在Trie里作为一个完整的 word
* 查找一个 Prefix String 是否存在于Trie里，且返回其最后一个node，如果false，则返回null
* 返回一个 Prefix String 在Trie里的所有 后继(Subsequent) Strings，用 List<String> 来表示

### 实现方式
#### 对于每个TrieNode的Children，可用以下方式中的一种
* `Map<Character, TrieNode>`
* `TrieNode[26]`，26的意思是26个小写字母。注意这里是array of TrieNode，不是array of char 或 int。这种方式也不存char，用数组里的相对位置就知道一个child node的val是哪个char了
  * Trie的层数多了以后，这种方式比Map的方式要快很多，也要省很多空间

#### 对于Trie class的member methods
* 可以用Recursive的方式来写
* 也可以用Iterative的方式来写。下面分别用了这两种方式

### Complexity
* Time:
  * 找一个String是否存在：O(这个String的长度)
  * 找某个String里的某个字母后面有没有接另外一个字母，或者它后面有哪些字母：O(1)
* Space: O(所有Strings的长度和)

## Implementation 1: Children用`Map<Character, TrieNode>`表示. Member Methods用Recursion写

### Java
```java
class TrieNode {
    char val;
    Map<Character, TrieNode> children;
    boolean endOfWord; // end of word以后，后面还可能继续有别的word！例：Trie里同时存 car 和 card 的情况
    int size; // number of all descendants, including this node itself if it is an end of a word
    
    public TrieNode(char c) {
        this.val = c;
        this.children = new HashMap<>();
    }
}

public class Trie {
    TrieNode root;
    
    public Trie() {
        this.root = new TrieNode('');
    }
    
    // insert a word into the Trie
    // ----------------------------------------------------------------
    public void insert(String word) {
        insert(root, word, 0);
        root.size ++;
    }
    
    private void insert(TrieNode node, String word, int index) {
        if (index == word.length()) {
            return;
        }
        
        char c = word.charAt(index);
        TrieNode child = node.children.get(c);
        
        if (child == null) {
            child = new TrieNode(c);
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
    
    private boolean search(TrieNode node, String word, int index) {
        char c = word.charAt(index);
        TrieNode child = node.children.get(c);
        
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

    // Returns the ending Node if the given Prefix String exists in the Trie,
    // return null if the Prefix String doesn't exist in the Trie
    // 这个method和search整个word的method基本一样，唯一区别是这个method不判断是否到了end of word
    // ----------------------------------------------------------------
    public TrieNode getEndNodeOfPrefixString(String prefix) {
        return findEndNode(root, prefix, 0);
    }
    
    private TrieNode findEndNode(TrieNode node, String prefix, int index) {
        char c = prefix.charAt(index);
        TrieNode child = node.children.get(c);
        
        if (child == null) {
            return null;

        if (index == prefix.length() - 1) {
            return child;
        }
        
        return findEndNode(child, prefix, index + 1);
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
    
    private boolean delete(TrieNode node, String word, int index) {
        char c = word.charAt(index);
        TrieNode child = node.children.get(c);
        
        if (child == null) {
            return false;
        }
        
        child.size--;
        
        if (index == word.length() - 1) {
            if (child.endOfWord == true) {
                child.endOfWord = false;
                
                // 这一步操作 可有可无
                if (child.size == 0) {
                    node.children.remove(c);
                }
            
                return true;
            }
            return false;
        }
        
        return delete(child, word, index + 1);
    }
    
    // Returns all the subsequent words starting with this prefix, in a list of Strings,
    // if the prefix itself is also a complete word in the Trie, then include itself.
    // 这个method的做法是，先找到prefix string的end node在trie里的位置，再找
    // 从这个end node开始往下的所有 substrings。再把这些 substrings 和 prefix 连在一起
    // ----------------------------------------------------------------
    public List<String> getAllWordsWithPrefix(String prefix) {
        List<String> result = new ArrayList<>();
        
        TrieNode endNode = getEndNode(prefix);
        if (endNode == null) {
            return result;
        }
        
        List<String> substrings = new ArrayList<>();
        getAllSubstrings(endNode, new StringBuilder(), substrings);
        
        for(String sub : substrings) {
            result.add(prefix + sub);
        }
        return result;
    }
    
    private void getAllSubstrings(TrieNode node, StringBuilder sb, List<String> substrings) {
        if (node.endOfWord) {
            // 如果当前node是一个word的end，就要把这个word记录到substrings里面去，
            // 但是在此不要return。DFS必须继续下去，因为后面可能还连着更长的词
            substrings.add(sb.toString());
            // return; <== do not return here!
        }
    	
        // Map.values(): returns a Collection view of the values contained in this map.
        for (TrieNode child : node.children.values()) {
            sb.append(child.val);
            getAllSubstrings(child, sb, substrings);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```

## Implementation 2: Children用`TrieNode[26]`表示. Member Methods用Iteration写

### Java
```java
class TrieNode {
    // this Node class does not have a "value" property!
    TrieNode[] children;
    boolean endOfWord;
    int size; // number of all descendants, including this node itself if it is an end of a word
    
    // this constructor does not need a value to be input
    public TrieNode() {
        this.children = new TrieNode[26]; // from 'a' to 'z'
    }
}

class Trie {
    TrieNode root;
    
    public Trie() {
        this.root = new TrieNode();
    }
    
    // Insert a word into the Trie
    // ----------------------------------------------------------------
    public void insert(String word) {
        TrieNode curNode = this.root;
        
        for (char c : word.toCharArray()) {
            if (curNode.children[c - 'a'] == null) {
                curNode.children[c - 'a'] = new TrieNode();
            }
            // 如果c和以前的所有chars都已经在Trie里了，则do nothing
            
            curNode.size++;
            curNode = curNode.children[c - 'a'];
        }
        
        curNode.size++;
        curNode.endOfWord = true;
    }
    
    // Search for a word in the Trie
    // ----------------------------------------------------------------
    public boolean search(String word) {
        TrieNode curNode = this.root;
        
        for (char c : word.toCharArray()) {
            if (curNode.children[c - 'a'] == null) {
                return false;
            }
            curNode = curNode.children[c - 'a'];
        }
        
        if (!curNode.endOfWord) {
            return false;
        }
        return true;
    }
    
    // Returns the ending Node if the given Prefix String exists in the Trie,
    // return null if the Prefix String doesn't exist in the Trie
    // ----------------------------------------------------------------
    public TrieNode getEndNodeOfPrefixString(String prefix) {
        TrieNode curNode = this.root;
        
        for (char c : prefix.toCharArray()) {
            if (curNode.children[c - 'a'] == null) {
                return null;
            }
            curNode = curNode.children[c - 'a'];
        }
        return curNode;
    }
    
    // Delete a word in a Trie
    // ----------------------------------------------------------------
    public boolean delete(String word) {
        if (!search(word)) {
            return false;
        }
        
        TrieNode curNode = this.root;
        for (char c : word.toCharArray()) {
            curNode.size--;
            curNode = curNode.children[c - 'a'];
        }
        
        curNode.size--;
        curNode.endOfWord = false;
        return true;
    }
    
    // Returns all the subsequent words starting with this prefix, in a list of Strings,
    // if the prefix itself is also a complete word in the Trie, then include itself.
    // ----------------------------------------------------------------
    public List<String> getAllWordsWithPrefix(String prefix) {
        List<String> result = new ArrayList<>();
        
        TrieNode endNodeOfPrefix = getEndNodeOfPrefixString(prefix);
        if (endNodeOfPrefix == null) {
            return result;
        }
        
        List<String> substrings = new ArrayList<>();
        getAllSubstrings(endNodeOfPrefix, new StringBuilder(), substrings);
        
        for (String s : substrings) {
            result.add(prefix + s);
        }
        return result;
    }
    
    private void getAllSubstrings(TrieNode node, StringBuilder sb, List<String> substrings) {
        if (node.endOfWord) {
            substrings.add(sb.toString());
        }
        
        for (int i = 0; i < 26; i++) {
            TrieNode child = node.children[i];
            if (child == null) {
                continue;
            }
            
            sb.append((char)('a' + i));
            getAllSubstrings(child, sb, substrings);
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}

public class Solution {
    public static void main(String[] args) {
        Trie trie = new Trie();
        trie.insert("ab");
        trie.insert("ac");
        
        System.out.println(trie.getAllWordsWithPrefix("a")); // [ab, ac]
    }
}  
```

## Reference
* [Trie | Insert and Search [GeeksforGeeks]](https://www.geeksforgeeks.org/trie-insert-and-search/)
* [Trie | Delete [GeeksforGeeks]](https://www.geeksforgeeks.org/trie-delete/)

{% include links.html %}
