---
title: "2 Sum: Input is a BST"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_bst.html
folder: algorithm
toc: false
---

## Description
Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.
```java
 public class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```
这是一道 easy 档次的题目

### Example
* Input: target = 9
  ```
      5
     / \
    3   6
   / \   \
  2   4   7
  ```
  * Output: True

## Solution
1. 既然是BST，那么用inorder traversal，把所有nodes的values都弄出来，自然会形成一个排好序的list（因为事先不知道size，所以用list比用array方便。得到所有数以后，也不必把list转成array，two sum 用set来做的话可以直接在list上做，不必前后2个指针那样非要在array上做）
2. 在这个sorted list 上做 two sum，用hashset的方法。见下面的代码

### Complexity
* Time: O(n)，n 是tree里nodes的个数
* Space: O(n)，stack

### Java
代码虽然看起来有点长，其实逻辑非常简单
```java
class Solution {
    public boolean findTarget(TreeNode root, int target) {
        if (root == null) {
            return false;
        }

        return twoSum(inorderTraversal(root), target);
    }
    
    private List<Integer> inorderTraversal(TreeNode root) {
        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);
        List<Integer> list = new ArrayList<>();
        
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            
            if (cur.left == null && cur.right == null) {
                list.add(cur.val);
                continue;
            }
            
            // inorder，需要先放right到stack里去，因为要先搞left
            if (cur.right != null) {
                stack.push(cur.right);
            }
            stack.push(cur);
            if (cur.left != null) {
                stack.push(cur.left);
            }
            
            cur.left = null;
            cur.right = null;
        }
        return list;
    }
    
    private boolean twoSum(List<Integer> nums, int target) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            if (set.contains(target - num)) {
                return true;
            }
            set.add(num);
        }
        return false;
    }
}
```

## Reference
* [Two Sum IV - Input is a BST [LeetCode]](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/)

{% include links.html %}
