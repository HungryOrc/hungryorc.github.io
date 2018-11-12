---
title: "All the Nodes of Distance K to Target Node in Binary Tree"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_nodes_of_distance_k_to_target_in_binary_tree.html
folder: algorithm
toc: false
---

## Description
We are given a binary tree (with root node `root`), a `target` node, and an integer value `k`.

Return a list of the values of all nodes that have a distance `k` from the `target` node.  The answer can be returned in any order.
```java
 public class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode(int x) { val = x; }
 }
```
Notice:
* The given tree is non-empty.
* Each node in the tree has **unique values** 0 <= node.val <= 500.
* The target node **is an existing node** in the tree.
* 0 <= K <= 1000.

### Example
* Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2
  * Output: [7, 4, 1]
  * Explanation: The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.
    * Note that the inputs "root" and "target" are actually TreeNodes. The descriptions of the inputs above are just serializations of these objects.

## Solution
首先是把tree变成一个graph！从root开始往下遍历所有的nodes。把每个node的所有neighbors，包括其parent node和direct children nodes，
都放到它的class里面。
由于树的性质，在这个过程里，一定不会重复放neighbors，所以neighbors用list装就行，不必用set装。

因为题目说了所有node的val都各不相同，所以我们用hashset去重的时候可以往里面装int value，而不是node object，这样我们就不需要给 node class 重写 
`hashCode`和`equals`这两个method 

### Complexity
* Time: O(n)，n 是tree里面nodes的个数
* Space: O(n)，新建的map

### Java
```java
class Solution {
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        List<Integer> result = new ArrayList<>();
        if (k < 0) {
            return null;
        }
        if (k == 0) {
            result.add(target.val);
            return result;
        }
        
        Map<TreeNode, List<TreeNode>> neighbors = new HashMap<>();
        Set<TreeNode> visited = new HashSet<>();
        
        neighbors.put(root, new LinkedList<TreeNode>());
        fillMap(root, neighbors);
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(target);
        visited.add(target);
        
        int level = 0;
        while (level < k) {
            level++;
            int size = queue.size();
            
            for (int i = 0; i < size; i++) {
                TreeNode curNode = queue.poll();
                
                for (TreeNode nei : neighbors.get(curNode)) {
                    if (!visited.contains(nei)) {
                        queue.offer(nei);
                        visited.add(nei);
                    }
                }
            }
        }
        
        // now level = k
        while (!queue.isEmpty()) {
            result.add(queue.poll().val);
        }
        return result;
    }
    
    private void fillMap(TreeNode node, Map<TreeNode, List<TreeNode>> neighbors) {
        if (node.left != null) {
            neighbors.get(node).add(node.left);
            
            List<TreeNode> neis = new LinkedList<>();
            neis.add(node);
            neighbors.put(node.left, neis);
            
            fillMap(node.left, neighbors);
        }
        
        if (node.right != null) {
            neighbors.get(node).add(node.right);
            
            List<TreeNode> neis = new LinkedList<>();
            neis.add(node);
            neighbors.put(node.right, neis);
            
            fillMap(node.right, neighbors);
        }        
    }
}
```

## Reference
* [All Nodes Distance K in Binary Tree [LeetCode]](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/description/)

{% include links.html %}
