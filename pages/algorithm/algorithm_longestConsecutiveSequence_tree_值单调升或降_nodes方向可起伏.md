---
title: "Longest Consecutive Sequence in Binary Tree, 值单调升或降，nodes方向可起伏"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longestConsecutiveSequence_tree_值单调升或降_nodes方向可起伏.html
folder: algorithm
toc: false
---

## Description
* tree里的 consecutive sequence 的意思是 **位置相邻** 且 **value连续变化(只差1)** 的nodes
  * 这和 array 里的 consecutive sequence 的意思是不一样的，array里是要求 **位置不一定相邻**，**value连续变化(只差1)** 的elements
* “值单调升或降”的意思是这里的 consecutive sequence 里的值必须是后一个比前一个大1，或者后一个比前一个小1。要么一直增大，要么一直减少
* node方向可起伏，即 可以从parent到child，也可以从child到parent，也可以从child到parent再到child

Given a binary tree, you need to find the length of Longest Consecutive Path in Binary Tree.

Especially, this path can be either increasing or decreasing. For example, [1,2,3,4] and [4,3,2,1] are both considered valid, but the path [1,2,4,3] is not valid. On the other hand, the path can be in the child-Parent-child order, where not necessarily be parent-child order.

Note: All the values of tree nodes are in the range of [-1e7, 1e7].
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### Example
* Input: 
  ```
        1
       / \
      2   3
  ```
  * Output: 2, The longest consecutive path is [1, 2] or [2, 1].
* Input: 
  ```
        2
       / \
      1   3
  ```
  * Output: 3, The longest consecutive path is [1, 2, 3] or [3, 2, 1].

## Solution 1：我自己的方法，用2个method，一个查进过cur node的 parent->child型路径，一个查以cur node为最高点的 child->parent->child型路径，速度意外地也挺快。后面有更好的方法
这个方法里有大量的重复计算，特别是用于 “查child->parent->child型路径” 的那个method，是严重的重复，在第一层要算2到h层，在第二层要算3到h层，
在第三层要算4到h层......

### Complexity
* Time: <=== ？？？
* Space: <=== ？？？ 

### Java
```java
class Solution {
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int[] max = {1};
        dfs(root, Integer.MIN_VALUE, false, 0, max);
        return max[0];
    }
    
    // 检查 parent->child 形状的path
    private void dfs(TreeNode curNode, int prevVal, boolean ascending, 
                     int prevLen, int[] max) {
        if (curNode == null) {
            return;
        }
        
        // step 1，看从parent node到cur node是否能延续 升序或降序
        int curVal = curNode.val;
        
        if (prevVal + 1 == curVal) {
            int curLen = 2; // prevNode 和 curNode 算是 len为2
            if (ascending) {
                curLen = prevLen + 1;
            }
            max[0] = Math.max(max[0], curLen);
            
            // 继续往下查 升序
            dfs(curNode.left, curVal, true, curLen, max);
            dfs(curNode.right, curVal, true, curLen, max);
        }
        else if (prevVal - 1 == curVal) {
            int curLen = 2; // prevNode 和 curNode 算是 len为2
            if (!ascending) {
                curLen = prevLen + 1;
            }
            max[0] = Math.max(max[0], curLen);

            // 继续往下查 降序
            dfs(curNode.left, curVal, false, curLen, max);
            dfs(curNode.right, curVal, false, curLen, max);
        }
        else { // 如果没有形成任何连续关系，但是当前node本身就构成了 curLen = 1
            dfs(curNode.left, curVal, false, 1, max);
            dfs(curNode.right, curVal, false, 1, max);            
        }
        
        
        // step 2, 当前node为顶点，左降右升，或者左升右降
        int leftAscendRightDescend = getSingleLen(curNode.left, curVal, true) + 
                                     getSingleLen(curNode.right, curVal, false);
        int leftDescendRightAscend = getSingleLen(curNode.right, curVal, true) + 
                                     getSingleLen(curNode.left, curVal, false);
        max[0] = Math.max(max[0], 1 + Math.max(leftAscendRightDescend, 
                                               leftDescendRightAscend));
    }
    
    // 检查 child->parent->child 形状的path
    private int getSingleLen(TreeNode node, int prevVal, boolean ascending) {
        if (node == null) {
            return 0;
        }
        
        int diff = 1;
        if (!ascending) {
            diff = -1;
        }
        if (prevVal != node.val + diff) {
            return 0;
        }
        
        return 1 + Math.max(getSingleLen(node.left, node.val, ascending), 
                            getSingleLen(node.right, node.val,ascending));
    }
}
```

## Solution 2：很巧妙的方法！最终的最长sequence一定有一个最高点，不管这个sequence是一条直线还是一条A型线，所以对于tree里的每个node我们都考察它作为最高点的话最长的sequence是多长就行了
以cur node 为顶点的2种类型的path都可以用一种方式来算，就是：
```
以cur node为顶点的最长的向下递增序列的长度 + 以cur node为顶点的最长的向下递减序列的长度 - 1
```
要注意，以cur node为顶点的递增和递减序列不可能重合，即cur node的左/右 children要么是递增的一员，要么是递减的一员！所以 **计算的时候不必区分
左边递增/递减 还是右边递增/递减，直接对左右child都计算递增/递减序列的长度就行了，具体见下面的code**

### Complexity
* Time: O(n) <=== ？？？
* Space: O(height of tree) <=== ？？？ 

### Java
```java
class Solution {
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int[] max = {1};
        dfs(root, max);
        return max[0];
    }
    
    // 返回的数组长度为2，里面的
    // 第一个int是以cur node 为顶点，包括它在内，最长的向下递增序列的长度
    // 第二个int是....................................递减........
    private int[] dfs(TreeNode curNode, int[] max) {
        if (curNode == null) {
            return new int[]{0, 0};
        }
        
        int curVal = curNode.val;
        int[] result = {1, 1};
        
        if (curNode.left != null) {
            int[] resultL = dfs(curNode.left, max);
            if (curVal + 1 == curNode.left.val) {
                result[0] = Math.max(result[0], resultL[0] + 1);
            } else if (curVal - 1 == curNode.left.val) {
                result[1] = Math.max(result[1], resultL[1] + 1);
            }
        }
        if (curNode.right != null) {
            int[] resultR = dfs(curNode.right, max);
            if (curVal + 1 == curNode.right.val) {
                result[0] = Math.max(result[0], resultR[0] + 1);
            } else if (curVal - 1 == curNode.right.val) {
                result[1] = Math.max(result[1], resultR[1] + 1);
            }
        }
        
        max[0] = Math.max(max[0], result[0] + result[1] - 1);
        
        return result;
    }
}
```

## Reference
* [Binary Tree Longest Consecutive Sequence II
 [LeetCode]](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/description/)

{% include links.html %}
