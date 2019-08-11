---
title: "Range Sum Query - Mutable"
tags: [algorithm, segment_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rangeSumQuery_mutable.html
folder: algorithm
toc: false
---

## Description
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.
```java
// Your NumArray object will be instantiated and called as such:
NumArray obj = new NumArray(nums);
obj.update(i,val);
int param_2 = obj.sumRange(i,j);
```

Note:
* The array is only modifiable by the update function.
* You may assume the number of calls to update and sumRange function is distributed evenly.

### Example
* Input: nums = [1, 3, 5]
  * Output: 
    ```
    sumRange(0, 2) -> 9
    update(1, 2)
    sumRange(0, 2) -> 8
    ```

## Solution 1: 我的方法，Segment Tree 我自己的实现。速度 前60%，有点慢，回头看看别人的方法

### Complexity
* Time: O(nlogn) <==== ？？？？
* Space: O(n) <==== ？？？？ 树的size，n 是nodes个数，也是nums里的元素的个数

### Java
```java
class SegTreeNode {
    int indexL, indexR;
    int sum;
    SegTreeNode nodeL, nodeR;
    
    public SegTreeNode(int il, int ir, int sum) {
        indexL = il;
        indexR = ir;
        this.sum = sum;
    }
}

class SegTree {
    int[] nums;
    SegTreeNode root;
    
    public SegTree(int[] nums) {
        this.nums = nums;
        
        int n = nums.length;
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        
        this.root = new SegTreeNode(0, n - 1, 0);
        initialPopulateTree(this.root, this.nums);
    }
    
    private void initialPopulateTree(SegTreeNode root, int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            add(root, i, nums[i]);
        }
    }
    
    public void add(SegTreeNode node, int i, int val) {
        node.sum += val;
        
        int il = node.indexL;
        int ir = node.indexR;
        
        if (il == ir) {
            return;
        }
        
        // il到mid算 左子树，mid+1到ir算 右子树
        int mid = il + (ir - il) / 2;
        
        if (i <= mid) {
            if (node.nodeL == null) {
                SegTreeNode nodeL = new SegTreeNode(il, mid, 0);
                node.nodeL = nodeL;
            }
            add(node.nodeL, i, val);
        } else {
            if (node.nodeR == null) {
                SegTreeNode nodeR = new SegTreeNode(mid + 1, ir, 0);
                node.nodeR = nodeR;
            }
            add(node.nodeR, i, val);
        }
    }
    
    public void update(int i, int val) {
        int difference = val - nums[i];
        this.nums[i] = val; // 这个别忘了
        
        add(root, i, difference);
    }
    
    public int sumRange(int i, int j) {
        return getSum(root, i, j);
    }
    
    private int getSum(SegTreeNode node, int i, int j) {
        if (i == node.indexL && j == node.indexR) {
            return node.sum;
        }
        
        int mid = node.indexL + (node.indexR - node.indexL) / 2;
        if (j <= mid) {
            return getSum(node.nodeL, i, j);
        } else if (i >= mid + 1) {
            return getSum(node.nodeR, i, j);
        } else {
            return getSum(node.nodeL, i, mid) + getSum(node.nodeR, mid + 1, j);
        }
    }
}

public class NumArray {
    SegTree sg;

    public NumArray(int[] nums) {
        this.sg = new SegTree(nums);
    }
    
    public void update(int i, int val) {
        this.sg.update(i, val);
    }
    
    public int sumRange(int i, int j) {
        return sg.sumRange(i, j);
    }
}
```

## Reference
* [Range Sum Query - Mutable [LeetCode]](https://leetcode.com/problems/range-sum-query-mutable/description/)

{% include links.html %}
