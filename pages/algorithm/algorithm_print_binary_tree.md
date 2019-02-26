---
title: "Print Binary Tree with Same Width in Each Row"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_print_binary_tree.html
folder: algorithm
toc: false
---

## Description
Print a binary tree in an m*n 2D string array following these rules:
* The row number m should be equal to the height of the given binary tree.
* The column number n should always be an odd number.
* The root node's value (in string format) should be put in the exactly middle of the first row it can be put. The column and the row where the root node belongs will separate the rest space into two parts (left-bottom part and right-bottom part). You should print the left subtree in the left-bottom part and print the right subtree in the right-bottom part. The left-bottom part and the right-bottom part should have the same size. Even if one subtree is none while the other is not, you don't need to print anything for the none subtree but still need to leave the space as large as that for the other subtree. However, if two subtrees are none, then you don't need to leave space for both of them.
* Each unused space should contain an empty string "".
* Print the subtrees following the same rules.
* The height of binary tree is in the range of [1, 10].

```java
 public class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```

这题看起来有些麻烦，想明白了就没啥。关键是一层一层画一下图！把每层每个点的间隔的规律找出来就好。认真画三四层以后可以发现：
**每个点的左右都有一样宽度的padding，当然这个padding是随着层数越往下而越小的，
然后每两个padding之间还要夹一个空位来表示上一层/两层/三层...的sub root，关键在这里可能会觉得繁琐，但是认真画图以后可以发现，
不管是多少层之上的sub root，往下投影的时候，层层叠叠，最后造成的效果都是每2个node之间（也就是每两个padding之间）正好
有且仅有一个sub root！**

### Example
* Input: 
  ```
     1
    /
   2
  ```
  * Output: 
    ```
    [["", "1", ""],
     ["2", "", ""]]
    ```

* Input: 
  ```
     1
    / \
   2   3
    \
     4
  ```
  * Output: 
    ```
    [["", "", "", "1", "", "", ""],
     ["", "2", "", "", "", "3", ""],
     ["", "", "4", "", "", "", ""]]
    ```
     
* Input: 
  ```
        1
       / \
      2   5
     / 
    3 
   / 
  4 
  ```
  * Output: 
    ```
    [["",  "",  "", "",  "", "", "", "1", "",  "",  "",  "",  "", "", ""]
     ["",  "",  "", "2", "", "", "", "",  "",  "",  "",  "5", "", "", ""]
     ["",  "3", "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]
     ["4", "",  "", "",  "", "", "", "",  "",  "",  "",  "",  "", "", ""]]
    ```

## Solution：我自己的土办法。速度 5%
先找到整棵树的height。然后每一层按照画图找出来的规律来从左到右填写

### Complexity
* Time: O(n) <=== 对么 ？？？
* Space: O(n)，queue size <=== 对么？？？

### Java
代码看起来长，其实逻辑不复杂。之后可以把代码再精简下，把可以重复使用的代码段提取出来作为helper functions
```java
class Solution {
    public List<List<String>> printTree(TreeNode root) {
        List<List<String>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        
        int height = getHeight(root);
        
        // for the 1st row
        List<String> list = new ArrayList<>();
        int padding = (int)Math.pow(2, height - 1) - 1;
        for (int i = 0; i < padding; i++) {
            list.add("");
        }
        list.add(root.val + "");
        for (int i = 0; i < padding; i++) {
            list.add("");
        }
        result.add(list);
        
        Queue<TreeNode> queue = new LinkedList<>();
        if (root.left != null) {
            queue.offer(root.left);
        } else {
            queue.offer(null);
        }
        if (root.right != null) {
            queue.offer(root.right);
        } else {
            queue.offer(null);
        }
        
        height--;
        while (height > 0) {
            int size = queue.size();
            list = new ArrayList<>();
            
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                
                padding = (int)Math.pow(2, height - 1) - 1;
                for (int j = 0; j < padding; j++) {
                    list.add("");
                }
                
                if (cur == null) {
                    list.add("");
                    queue.offer(null);
                    queue.offer(null);
                } else {
                    list.add(cur.val + "");
                    queue.offer(cur.left);
                    queue.offer(cur.right);
                }
                
                for (int j = 0; j < padding; j++) {
                    list.add("");
                }
                
                if (i != size - 1) {
                    list.add("");
                }
            }
            result.add(list);
            
            height--;
        }
        return result;
    }
    
    private int getHeight(TreeNode root) {
        int height = 0;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            height++;
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                if (cur.left != null) {
                    queue.offer(cur.left);
                }
                if (cur.right != null) {
                    queue.offer(cur.right);
                }
            }
        }
        return height;
    }
}
```

## Reference
* [Print Binary Tree [LeetCode]](https://leetcode.com/problems/print-binary-tree/description/)

{% include links.html %}
