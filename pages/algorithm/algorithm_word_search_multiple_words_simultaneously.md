---
title: "Word Search: Find Max Number of Words Simultaneously in a Matrix"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_word_search_multiple_words_simultaneously.html
folder: algorithm
toc: false
---

## Description
给一个矩阵 char[][] board，给一个数组 String[] words，求在board里同时能够最多存在多少个word里面的词。
board和words里面都只有小写字母a到z，没有任何其他char。
“同时存在”是指任何词的任何字母都不能在matrix上重叠。一个词可以在matrix上沿着上下左右四个方向进行延伸。

这题与Word Search II相似。但是Word Search II是在board里每次找words里的一个词。这题是要看board里最多能同时存在words里的几个词。
这题的难点在于 **在找到一个词以后，boolean[][] visited矩阵要把这个找到的词的各个chars的位置标记好；但另一方面，在找一个词的过程中，
它的各个char每次被添加到visited矩阵里以后，都要在适当的时候做back tracking，即从visited矩阵里剔除出去**。

### Example
* Input: String[] dict = {"abc","deh","cfi","eh", "xy", "fi", "f"}
  ```
  char[][] board = {{'a', 'b', 'c'},
                    {'d', 'e', 'f'},
                    {'g', 'h', 'i'}};
  ```
  * Output: 3, for example a max size set is: {"abc", "deh", "fi"}

## Solution: DFS，特殊之处在于 DFS函数里面还有双层for循环！
“DFS函数的内部结构也许看起来疯狂，但其实 只要当前的状态越来越接近 Base Case，就不会无限循环，就没错”

步骤
* 对于矩阵上从从左到右从上到下的每一点，用DFS找有没有从它开始的word
  * 没有的话，就去矩阵上的下一个点作为新的开始点，按上面说的从左到右从上到下的顺序
  * 如果找到了，就在visited矩阵上标记这个词的各个chars的位置，然后开始找下一个词。每找到一个词要记得更新最终的max size of found words
* 在找每个词的过程中，要记得做visited矩阵上的back tracking，即 标记和取消标记
* 用Trie能大幅提高找词的速度。不过Trie并不是本解法的核心逻辑所在，不是必须

### Complexity
* Time: O(buildTrieTime + (wordAvgLen) * 2^(n * m)) <==== Jack老师给的时间复杂度，但我还是不理解
* Space: O(n * m), dp矩阵

### Java
```java
class TrieNode {
    // this kind of Node class does not have a "value" property
    TrieNode[] children;
    boolean endOfWord;

    public TrieNode() {
        this.children = new TrieNode[26]; // from 'a' to 'z'
    }
}

class Trie {
    TrieNode root;
    
    public Trie() {
        this.root = new TrieNode(); // root node does not hold any char
    }
    
    public void insert(String word) {
        TrieNode curNode = this.root;
        
        for (char c : word.toCharArray()) {
            if (curNode.children[c - 'a'] == null) {
                curNode.children[c - 'a'] = new TrieNode();
            }
            curNode = curNode.children[c - 'a'];
        }
        curNode.endOfWord = true;
    }
}

public class Solution {
    private static final int[][] DIRS = {
        {1, 0}, {-1, 0}, {0, 1}, {0, -1}
    };
    
    public int largestCollection(char[][] board, String[] dict) {
        // ignore validations of the parameters
        
        int n = board.length;
        int m = board[0].length;
        
        // Build Trie, time cost: sum of lengths of the words in the dict
        Trie trie = new Trie();
        for (String word : dict) {
            trie.insert(word);
        }
        
        int[] maxSize = {0}; // an array to carry around the max size of the collection
        boolean[][] visited = new boolean[n][m];
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                dfsForMultipleWords(board, trie.root, trie.root, i, j, visited, 0, maxSize);
            }
        }

        return maxSize[0];
    }
    
    private void dfsForMultipleWords(char[][] board, TrieNode root, TrieNode curNode, 
            int x, int y, boolean[][] visited, int curSize, int[] maxSize) {
        if (!valid(x, y, board)) {
            return;
        }
        if (visited[x][y]) {
            return;
        }
        
        char nextChar = board[x][y];
        if (curNode.children[nextChar - 'a'] == null) {
            return;
        }
        
        curNode = curNode.children[nextChar - 'a'];
        
        // mark visited of cur node
        visited[x][y] = true;
        
        // if we are currently at an end of a word，then we
        // keep this word to be marked as visited, and start to find the next word
        if (curNode.endOfWord) {
            curSize++;
            maxSize[0] = Math.max(maxSize[0], curSize);
            
            // dfs函数里再包一个 双层for loop！前所未闻！
            // 而且注意这个 双层for循环 是再次从0,0开始，从左到右，从上到下！
            for (int i = 0; i < board.length; i++) {
                for (int j = 0; j < board[0].length; j++) {
                    dfsForMultipleWords(board, root, root, i, j, visited, curSize, maxSize);
                }
            }
        }
        else {
            for (int[] dir : DIRS) {
                int newX = x + dir[0];
                int newY = y + dir[1];
                
                dfsForMultipleWords(board, root, curNode, 
                        newX, newY, visited, curSize, maxSize);
            }
        }
        
        // 复原
        visited[x][y] = false;
    }
    
    private boolean valid(int x, int y, char[][] board) {
        int n = board.length, m = board[0].length;
        return x >= 0 && x < n && y >= 0 && y < m;
    }

    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {
        String[] dict = {"abc","deh","cfi","eh", "xy", "fi", "f"};
        
        char[][] board = {
            {'a', 'b', 'c'},
            {'d', 'e', 'f'},
            {'g', 'h', 'i'}
        };
        
        Solution solu = new Solution();
        int result = solu.largestCollection(board, dict);
        
        System.out.println(result); // should be 3
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
