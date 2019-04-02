---
title: "Subsets of an Array of (Possibly) Duplicate Elements"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_subsetsOfArray_DuplicateElements.html
folder: algorithm
toc: false
---

## Description
Given an array of **possibly duplicate** integers, return all possible distinct subsets.

Note: 
* The solution set must not contain duplicate subsets.

### Example
* Input: nums = [1,2,2]
  * Output: 
    ```
    [
      [2],
      [1],
      [1,2,2],
      [2,2],
      [1,2],
      []
    ]
    ```

## Solution：DFS, 用 Recursive Searching Tree 做。速度 前1%
思路：看下一个位置可以放哪些数，而非某一个数是否出现在当前的subset里
```       
                                []
                     /       /      \      \
                 [1]        [2]     2:不采用  [3]
	       /   \       /   \
	   [1,2] [1,3]   [2,2] [2,3]
	  /    \          /
  [1,2,2] [1,2,3]   [2,2,3]
    / 
[1,2,2,3]	
```

### Complexity
* Time: O(答案的个数 * 得到每个答案所需的时间) = O(2^n * n)
  * 得到每个答案所需的时间是 O(n)，是因为可能要把n个数都add到一个答案里去
* Space: O(n)，DFS的 call stack的层数 <=== 对么 ？？？？

### Java
```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return result;
        }
        
        // 对于有重复元素的数组nums，对于本方法，排序是必须的
        Arrays.sort(nums);
        
        dfs(nums, 0, new ArrayList<Integer>(), result);
        return result;
    }

    private void dfs(int[] nums, int curIndex, List<Integer> curList,
                     List<List<Integer>> result) {
        
	// 这题的一大特点：先什么都不管，先把curList扔到result里去
        result.add(new ArrayList<Integer>(curList)); // new一个
        
        // 下一个位置可以放哪些数
        for (int i = curIndex; i < nums.length; i++) {
	
	    // 本方法的最关键的一句在此！
	    // 这也是duplicate elements 和 distinct elements 解法的唯一不同之处！
            if (i > curIndex && nums[i] == nums[i - 1]) {
                continue;
            }
            
            curList.add(nums[i]);
            dfs(nums, i + 1, curList, result); // 这里是 i+1 不是 curIndex+1
            curList.remove(curList.size() - 1); // 复原
        }
    }
}
```

另一种和上面的思路一样的解法，只是dfs函数里的有些具体处理方式稍有不同，在下面的注释里标明了。速度仍然是 前1%
```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return result;
        }
        
        Arrays.sort(nums);
        
        dfs(nums, 0, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void dfs(int[] nums, int index, List<Integer> list, List<List<Integer>> result) {
        if (index == nums.length) { // 这里还是用传统的结束条件
            result.add(new ArrayList<Integer>(list));
            return;
        }
        
        for (int i = index; i < nums.length; i++) {
            if (i > index && nums[i] == nums[i - 1]) {
                continue;
            }
            
            list.add(nums[i]);
            dfs(nums, i + 1, list, result);
            list.remove(list.size() - 1);      
        }
        
        dfs(nums, nums.length, list, result); // 这个是为了别忘记空的list的情况，注意这里index直接填nums.length了
    }
}
```

## Reference
* [Subsets II [LeetCode]](https://leetcode.com/problems/subsets-ii/description/)

{% include links.html %}
