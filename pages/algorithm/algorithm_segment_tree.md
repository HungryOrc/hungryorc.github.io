---
title: "Segment Tree"
tags: [algorithm, algorithm_overview, segment_tree]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_segment_tree.html
folder: algorithm
toc: false
---

## Overview
在关注一个range里的min、max、count、sum之类的性质的时候，就可以考虑用 segment tree。但这也不是绝对的。如果 update data 之类的 modify 性质的操作较多，
则用 segment tree较好。如果modify类的操作较少或者没有，主要只是query类的操作，则可能用一个数组就可以了，还更快捷。

对于Segment Tree，它的每个TreeNode上一定会存一个Range，比如[int start, int end]，即它和它的所有子孙Nodes所处的Range。但是SegmentTreeNode上不会再存一个传统意义上的value，比如说一个SegmentTreeNode上存的Range是[1, 4]，那么它上面不会再存一个所谓的[1, 4]之间的int value，事实上[1, 4]本身就是这个SegmentTreeNode的**value**！

除了Range之外，SegmentTreeNode上还会存至少一个别的值，这些值也不是传统意义上的value，而是与Range相关的某种(甚至多种)属性，比如当前Range里的min、max、sum、count(这个count的意思是当前Range里有几个数据，虽然这些数据本身反而不出现在当前的TreeNode上)之类。

至于 Augment Tree (增强树)，它和 Segment Tree 都是“高级”版的树状结构。不过 Agument Tree 对于 range 类的问题没什么长处，它的增强主要在于对单个 tree node 的增强。

## Implementation 1: 每个TreeNode上存的是当前Range里的所有values的sum
这里用 SegmentTreeNode 来解决这样的题目：一个数组，要取任何一个闭区间上的sum(即query)，而且各个元素都可以被修改(即update)。

### Complexity
* Time: O(n logn)，因为对于每个元素，都要进行一次query，以及一次update。而一次query和一次update都是从树顶走到树叶，都是 logn 的时间
* Space: O(n)，因为要建这个树，这个树的leaves是O(n)的量级，leaves上的所有nodes加在一起的个数也是O(n)的量级，所以一共也是 O(n)的量级

### Java
```java
class SegmentTreeNode {
    int start, end; // range
    int sum; // sum of all the values in the range
    SegmentTreeNode left, right;
    
    public SegmentTreeNode(int start, int end, int sum) {
        this...
    }
}

public class NumArray {
    int[] nums;
    SegmentTreeNode root;
    
    public NumArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return;
        }
        
        this.nums = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            this.nums[i] = nums[i];
        }
        
        this.root = buildTree(this.nums, 0, nums.length - 1);
    }
    
    // i is index, val is the new value
    public void update(int i, int val) {
        int diff = val - nums[i];
        nums[i] = val;
        updateHelper(root, i, diff);
    }
    
    // 需要这个Helper的原因是 public API 的参数列表里不会含有SegmentTreeNode，因为
    // 用SegmentTree只是解决这个问题的多种方式之一，public API不可能规定要出现 SegmentTreeNode
    private void updateHelper(SegmentTreeNode root, int index, int diff) {
        if (root == null || index < root.start || index > root.end) {
            return;
        }
        
        root.sum += diff;
        
        // if root is a leaf node
        if (root.start == root.end) {
            return;
        }
        
        int mid = root.start + (root.end - root.start) / 2;
        if (index <= mid) { // index 在左半段
            updateHelper(root.left, index, diff);
        } else { // index 在右半段
            updateHelper(root.right, index, diff);
        }
    }
    
    // 这就是所谓的 query 函数
    public int sumRange(int start, int end) {
        if (this.root == null || start > end || 
            start > this.root.end || end < this.root.start) {
            return 0;
        }
        return sumRangeHelper(this.root, start, end);
    }

    private int sumRangeHelper(SegmentTreeNode root, int start, int end) {
        if (root == null || start > root.end || end < start.start) {
            return 0;
        }
        
        if (start <= root.start && end >= root.end) {
            return root.sum;
        }
        
        int mid = root.start + (root.end - root.start) / 2;
        if (end <= mid) { // 全在左半段
            return sumRangeHelper(root.left, start, end);
        } else if (start >= mid + 1) { // 全在右半段
            return sumRangeHelper(root.right, start, end);
        } else {
            return sumRangeHelper(root.left, start, mid) + sumRangeHelper(root.right, mid + 1, end);
        }
    }
    
    private SegmentTreeNode buildTree(int[] nums, int start, int end) {
        if (start > end) {
            return null;
        }
        // if a leaf node
        if (start == end) {
            return new SegmentTreeNode(start, end, nums[start]);
        }
        
        int mid = start + (end - start) / 2;
        SegmentTreeNode leftNode = buildTree(nums, start, mid);
        SegmentTreeNode rightNode = buildTree(nums, mid + 1, end);
        
        SegmentTreeNode curNode = new SegmentTreeNode(start, end, leftNode.sum + rightNode.sum);
        curNode.left = leftNode;
        curNode.right = rightNode;
        
        return curNode;
    }
} 
```

## Implementation 2: 每个TreeNode上存的是当前Range里的TreeNodes的个数count
这里用 SegmentTreeNode 来解决 Get Number of Smaller Elements after Self in an Array 这样的题目。

需要关注的是`int query(int start, int end, SegmentTreeNode root)`和`private void update(int val, SegmentTreeNode root)`这两个函数。它们是各类Segment Tree都必备的2个函数，与Segment Tree用来解决什么问题无关。

### Complexity
* Time: O(n logn)，因为对于每个元素，都要进行一次query，以及一次update。而一次query和一次update都是从树顶走到树叶，都是 logn 的时间
* Space: O(n)，因为要建这个树，这个树的leaves是O(n)的量级，leaves上的所有nodes加在一起的个数也是O(n)的量级，所以一共也是 O(n)的量级

### Java
```java
class SegmentTreeNode {
    int start, end; // range
    int count; // count of numbers in this range
    SegmentTreeNode left, right;
    
    public SegmentTreeNode(int start, int end) {
        this.start = start;
        this.end = end;
        // this.count will be set to zero automatically
    }
}

public Solution {
    
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> result = new ArrayList<>();
        
        int min = ... // min value in array nums
        int max = ... // max value in array nums   
        
        // from right to left of nums
        for (int i = nums.length - 1; i >= 0; i--) {
            // smaller than self means the number can be at most self - 1, 
            // and at least equals min of the array
            int numOfSmallerAfterSelf = query(min, nums[i] - 1, root);
            
            result.add(numOfSmallerAfterSelf); // 最后要反转顺序
            
            // 把当前nums[i]的“存在性”放到segment tree里去
            update(nums[i], root);
        }
        
        Collections.reverse(result);
        return result;
    }
    
    private int query(int start, int end, SegmentTreeNode root) {
        if (root == null || start > end) {
            return 0;
        }
        if (start <= root.start && end >= root.end) {
            return root.count;
        }
        
        int mid = root.start + (root.end - root.start) / 2;
        // 注意！这里默认的SegmentTreeNode的Range的两段的划分方式是：
        // `[start, mid]` 和 `[mid+1, end]`
        
        if (start >= mid + 1) { // 需要query的range在有半段
            return query(start, end, root.right);
        } else if (end <= mid) { // 需要query的range在左半段
            return query(start, end, root.left);
        } else { // 需要query的range横跨左右两半
            return query(start, mid, root.left) + query(mid + 1, end, root.right);
        }
    }
    
    private void update(int val, SegmentTreeNode root) {
        if (val > root.end || val < root.start) {
            return;
        }

        root.count ++;
        
        // root.start == root.end 就表示root本身是一个leaf node
        // 注意！这个statement必须在 root.count ++ 之后！即leaf本身的count也要+1，然后才算完
        if (root.start == root.end) {
            return;
        }
        
        int mid = root.start + (root.end - root.start) / 2;
        if (val <= mid) {
            if (root.left == null) {
                root.left = new SegmentTreeNode(root.start, mid);
            }
            
            update(val, root.left);
        } else {
            if (root.right == null) {
                root.right = new SegmentTreeNode(mid + 1, root.end);
            }
            
            update(val, root.right);
        }
    }
}
```

{% include links.html %}
