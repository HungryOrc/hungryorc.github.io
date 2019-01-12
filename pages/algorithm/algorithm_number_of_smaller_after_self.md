---
title: "Number of Smaller Numbers after Self in an Array"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_number_of_smaller_after_self.html
folder: algorithm
toc: false
---

## Description，这题较难
You are given an integer array `nums` and you have to return a new `counts` array. 
In the `counts` array, `counts[i]` is the number of smaller elements to the right of `nums[i]`.

### Example
* Input: nums = [5, 2, 6, 1]
  * Output: counts = [2, 1, 1, 0]

## Solution 1: 用 Merge Sort 来排序 indexes，思路很巧妙，值得认真消化
Ref: https://leetcode.com/problems/count-of-smaller-numbers-after-self/discuss/76583/11ms-JAVA-solution-using-merge-sort-with-explanation

我们要求的是，对于数组里的数nums[i]，要找数组里index > i，而且值小于nums[i]的数。
所以直接sort数组里的数是不行的，那样它们之间的顺序就丢失了，就算知道数组里有几个数小于nums[i]，我们也不知道这些小于它的数是否在它的右边。
所以我们转而考虑sort这些数的indexes。

另外我们用一个名为counts的数组表示nums里的每个数的右边有几个比它小的，比如counts[3] = 2 表示nums[3] 的右边有2个 比nums[3]小的数。

以数组nums = [1,6,3,2]为例。各个数的indexes自然是[0,1,2,3]，我们把这个称为 序数数组 indexes = [0,1,2,3]。在排序的过程中，每次需要swap数组nums里的两个数的时候，我们转而swap数组indexes里的两个index，而保持nums数组永远不变（永远哦）。每次我们要去找nums数组里的数的时候，我们就经由indexes数组里的index来找。我们用 merge sort 来实现整个过程，因为 merge sort 的 swap 过程比较清晰易懂：
* 每2个数之间的merge
  ```
  nums before merge:         [1][6] [3][2]
      nums should turn into: [1][6] [2][3], but we keep it intact, instead we merge the indexes array:
  indexes before merge:      [0][1] [2][3]
      indexes after merge:   [0][1] [3][2], 它们表征了nums数组里经过两两merge以后各个数的相对位置关系
  counts after merge:        [0][0] [1][0]
                                     ^
                                    +1
  ```
  * 在上面的过程里，nums数组里的1和6没动，所以对于它们来说，**意味着 在这一步里没有发现它们的右边有比它们(分别)小的数**
  * nums数组里的3和2对调了位置，2从3的右边调到了3的左边，所以对于3来说，**意味着 发现了它右边有1个比它小的数**，对于2来说，**意味着 这一步里没有发现它右边有比它小的数**
  * 所以在这一步里，对于nums数组里的四个数，**3所对应的count 应该 +1，其它数的count 不变**
* 每4个数之间的merge（具体解释见下面的文字，直接看下面的图不容易理解）
  ```
  nums array (actually we will never change them, we just swap and merge the indexes array):
      to be merged: [1][6]  [2][3] -> [6]  [2][3] -> [6]  [3]   -> [6]        -> completed
      merge result:                   [1]         -> [1][2]     -> [1][2][3]  -> [1][2][3][6]
  indexes array:
      to be merged: [0][1]  [3][2] -> [1]  [3][2] -> [1]  [2]   -> [1]        -> completed
      merge result:                   [0]         -> [0][3]     -> [0][3][2]  -> [0][3][2][1]
  counts array:
                    [0][0]  [1][0] -> [0][0][1][0]->[0][1][1][0]->[0][2][1][0]->[0][2][1][0]
                                                        ^             ^
                                                       +1            +1
  ```
  * 主要注意 counts 数组的变化过程。在这一步里，2个subarray:[1][6]和[2][3]要merge在一起，而这两个subarray本身都是sorted。那么merge过程就是谁小移谁。但如前所述，我们实质上是哪个num小，就移这个num的index，而非移这个num本身。在这个移动过程中，我们就要得到有多少个数在自己的右边而且比自己小。关键点如下，具体以此数组为例：
  * 首先要移左subarray里的num=1，即移index=0。
    * 因为num=1属于左subarray，所以它不会是任何右subarray里的数的“右小”数
    * 另一方面，num=1如果是左subarray里的任何数的“右小”数，那么这种关系一定已经反映在左subarray里的数的counts数组里了
    * 所以num=1的移动不会造成counts数组的任何变化
  * 然后移右subarray里的num=2，即移index=3。因为num=2属于右subarray，所以把它merge到总数组里的时候，**意味着左subarray里当前任何还没有被merge到总数组里的数，都存在num=2这么一个“右小”数，即当前左subarray里的这些数相对应的count值都要+1！**
    * 当前左subarray里只剩下num=6了（左subarray里的num=1在前一步里已经被merge到总数组里），所以num=6对应的index=1对应的counts[1]的值要 +1
    * 当前右subarray里的num=3的count值并不受num=2的移动的影响，因为当前在右subarray里，num=3在num=2的右边
  * 然后移右subarray里的num=3，即index=2。同理上一步，因为num=3属于右subarray，所以当前左subarray里剩下的所有数对应的index对应的count都要 +1
  * 最后移左subarray里的num=6，即index=1。同理前面的分析，它的移动不影响左右subarray里的任何数对应的index对应的count值
    
### Complexity
* Time: O(n logn)，merge sort 的时间消耗
* Space: O(n)

### Java
我写的code实现
```java
class Solution {
    
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> result = new ArrayList<>();
        
        if (nums == null || nums.length == 0) {
            return result;
        }
        
        int len = nums.length;
        int[] counts = new int[len];
        int[] indexes = new int[len];
        for (int i = 0; i < len; i++) {
            indexes[i] = i;
        }
        
        countSmallerByMergeSort(nums, 0, len - 1, indexes, counts);
        
        for (int count : counts) {
            result.add(count);
        }
        return result;
    }
    
    // 这个函数本身其实对于 indexes 和 counts 这两个数组没有任何改变，
    // 它只是为了把这两个数组的引用传给下一个函数 mergeAndCount 而已
    private void countSmallerByMergeSort(int[] nums, int left, int right,
                                        int[] indexes, int[] counts) {
        if (left >= right) {
            return;
        }
        
        int mid = left + (right - left) / 2;
        countSmallerByMergeSort(nums, left, mid, indexes, counts);
        countSmallerByMergeSort(nums, mid+1, right, indexes, counts);
        
        mergeAndCount(nums, left, mid, right, indexes, counts);
    }
    
    private void mergeAndCount(int[] nums, int left, int mid, int right,
                              int[] indexes, int[] counts) {
        int[] mergedIndexes = new int[right - left + 1];
        int numOfMergedIndexesInThisRound = 0;
        int movesFromRightSubarray = 0;
        
        int indexL = left;
        int indexR = mid + 1;
        
        while (indexL <= mid && indexR <= right) {
            // use <= instead of < here!! since if equal, then the right one 
            // does not count as the smaller and righter one of the left one!
            if (nums[indexes[indexL]] <= nums[indexes[indexR]]) {
                mergedIndexes[numOfMergedIndexesInThisRound] = indexes[indexL];
                
                // do this before indexL is incremented!!
                // update the counts array of the indexes of the left subarray
                counts[indexes[indexL]] += movesFromRightSubarray;
            
                indexL ++;
                numOfMergedIndexesInThisRound ++;
            } else {
                mergedIndexes[numOfMergedIndexesInThisRound] = indexes[indexR];
                
                movesFromRightSubarray++;
                
                indexR ++;
                numOfMergedIndexesInThisRound ++;
            }
        }
        
        while (indexL <= mid) { // right subarray is empty now
            mergedIndexes[numOfMergedIndexesInThisRound] = indexes[indexL];
            counts[indexes[indexL]] += movesFromRightSubarray;
            indexL ++;
            numOfMergedIndexesInThisRound ++;
        }
        
        while (indexR <= right) { // left subarray is empty now
            mergedIndexes[numOfMergedIndexesInThisRound] = indexes[indexR];
            indexR ++;
            numOfMergedIndexesInThisRound ++;          
        }
        
        // copy the section of the merged indexes back to the original indexes array
        for (int i = 0; i < mergedIndexes.length; i++) {
            indexes[left + i] = mergedIndexes[i];
        }
    }
}
```

## Solution 2: Segment Tree
Ref: Laicode

### Complexity
* Time: O(n logn)
* Space: O(n)

### Java
我的code
```java
class SegmentTreeNode {
  int start, end; // range
  int count; // count of numbers in this range
  SegmentTreeNode left, right;
  
  public SegmentTreeNode(int s, int e) {
    start = s;
    end = e;
  }
}

public class Solution {
  
  public List<Integer> countSmaller(int[] nums) {
    if (nums == null || nums.length == 0) {
      return null;
    }
    
    int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
    for (int num : nums) {
      if (num < min) {
        min = num;
      } else if (num > max) {
        max = num;
      }
    }

    SegmentTreeNode root = new SegmentTreeNode(min, max);
    
    List<Integer> result = new LinkedList<>();
    // 从右往左
    for (int i = nums.length - 1; i >= 0; i--) {
      int cur = nums[i];
      // smaller than self means the number can be at most self - 1, 
      // and at least equals min of the array
      int countOfSmaller = query(root, min, cur - 1);
      
      result.add(0, countOfSmaller); // add at head of linkedlist
      
      update(root, cur);
    }
    
    return result;
  }
  
  private int query(SegmentTreeNode node, int start, int end) {
    if (node == null || start > end) {
      return 0;
    }
    
    if (start <= node.start && node.end <= end) {
      return node.count;
    }
    
    // 注意！用node.start和node.end来算mid！不是用start和end来算mid
    int mid = node.start + (node.end - node.start) / 2;
    // 注意！这里默认的SegmentTreeNode的Range的两段的划分方式是：
    // `[start, mid]` 和 `[mid+1, end]`
    
    if (start >= mid + 1) { // 需要query的range在左半段
      return query(node.right, start, end);
    } else if (end <= mid) { // 需要query的range在有半段
      return query(node.left, start, end);
    }
    // 需要query的range横跨左右两半
    return query(node.left, start, mid) + query(node.right, mid + 1, end);
  }
  
  private void update(SegmentTreeNode node, int val) {
    if (val < node.start || val > node.end) {
      return;
    }
    
    node.count ++; // 别忘了这个！
    
    // root.start == root.end 就表示root本身是一个leaf node
    // 注意！这个statement必须在 root.count ++ 之后！即leaf本身的count也要+1，然后才算完
    if (node.start == node.end) {
      return;
    }
    
    int mid = node.start + (node.end - node.start) / 2;
    if (val <= mid) {
      // 发现没有的时候才“造node”！而不是一开始就把整个树都建全！
      if (node.left == null) {
        node.left = new SegmentTreeNode(node.start, mid);
      }
      update(node.left, val);
    } else {
      if (node.right == null) {
        node.right = new SegmentTreeNode(mid + 1, node.end);
      }
      update(node.right, val);
    }
  }
}
```

## Solution 3: 从尾部开始做 + ArrayList + 二分查找。这个方法思路很巧妙，但时间较慢，不采用，仅备案
Ref: https://discuss.leetcode.com/topic/31173/my-simple-ac-java-binary-search-code

对于数组里最右边的那个数，它右边比它小的数一定是0个，从它开始往左推导。Maintain an sorted ArrayList of numbers that had been visited. 

Use findIndex() to find the index of the 1st element in the sorted ArrayList which is larger or equal to the target number. 
For example, [5,2,3,6,1], by the time we reach 2, we should already have a sorted array: [1,3,6], 
findIndex() returns 1, which is the index where 2 should be inserted and is ALSO the number smaller than 2. 
Then we insert 2 into the sorted array to form [1,2,3,6], and set `counts[1]` to be 1 (relevant to the 2 at `nums[1]`).

Then keep doing this till the start of array `nums`.

### Complexity
* Time: O(n^2)
  * Every insertion into the Arraylist costs O(n) time, since it has to move all the subsequent elements one slot towards the end.
  * Use binary search in the ArrayList for each time cost only O(logn), but since insertion costs O(n), so finding and insertion together costs O(n).
* Space: O(n)

### Java
```java
public class Solution {
    
    public List<Integer> countSmaller(int[] nums) {
        Integer[] result = new Integer[nums.length];
        List<Integer> sortedAL = new ArrayList<>();
        
        // 从后往前！
        for (int i = nums.length - 1; i >= 0; i--) {
            int curNum = nums[i];
            int numOfSmallerNumsAfterSelf = getThe1stNumThatIsBiggerOrEqualToTargetInAL(curNum, sortedAL);
            
            // 前一个参数是插入的位置，后一个参数是插入的值
            sortedAL.add(numOfSmallerNumsAfterSelf, curNum);
            
            result[i] = numOfSmallerNumsAfterSelf;
        }
        return Arrays.asList(result); // 注意这个直接从数组转到ArrayList的函数！
    }
    
    // Binary Search
    // returns the index of the 1st number in the sorted arraylist that 
    // is bigger or equal to the target number
    private int getThe1stNumThatIsBiggerOrEqualToTargetInAL(int target, List<Integer> sortedAL) {
        if (sortedAL.size() == 0) {
            return 0;
        }
        
        int left = 0;     
        int right = sortedAL.size() - 1;
        
        // 特别注意！
        // 这里要特别处理一下后面的二分查找所无法解决的情况：就是整个查找区域里根本就没有我们要的案例.
        // 这一题我们要找第一个大于等于target的数，但也许整个sorted list里所有的数都小于target，
        // 如果是这样的话，如果还按照下面的二分的方式走，会return整个list的最末尾一位的index，
        // 但我们真正要的是插到list的最末尾一位的后面，而不是最末尾一位上，
        // 所以需要特别写一下下面这个 special case
        if (sortedAL.get(right) < target) {
            return right + 1;
        }
        
        while (left + 1 < right) {
            int mid = left + (right - left) / 2;
            int num = sortedAL.get(mid);
            
            if (num >= target) {
                right = mid;
            } else { // <
                left = mid + 1;
            }
        }
        
        if (sortedAL.get(left) >= target) {
            return left;
        } else {
            return right;
        }
    }
}
```

## Reference
* [Count of Smaller Numbers After Self [LeetCode]](https://leetcode.com/problems/count-of-smaller-numbers-after-self/description/)

{% include links.html %}
