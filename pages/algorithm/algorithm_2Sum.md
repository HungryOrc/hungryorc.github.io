---
title: "Two Sum: Input is a BST"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum.html
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
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
代码虽然看起来长，其实逻辑很简单
```java
class Solution {
    public boolean findTarget(TreeNode root, int target) {
        if (root == null) {
            return false;
        }
        
        int[] numbers = inorderTraversal(root);
        
        return twoSum(numbers, target);
    }
    
    private int[] inorderTraversal(TreeNode root) {
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
        
        int n = list.size();
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = list.get(i);
        }
        return result;
    }
    
    private boolean twoSum(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left < right && left < nums.length && right >= 0) {
            if (nums[left] + nums[right] > target) {
                right--;
            } else if (nums[left] + nums[right] < target) {
                left++;
            } else {
                return true;
            }
        }
        return false;
    }
}
```

## Reference
* [Two Sum IV - Input is a BST [LeetCode]](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/)

{% include links.html %}
