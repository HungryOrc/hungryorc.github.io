---
title: "Permutation with no Duplicate Elements"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_permutation_with_no_duplicate_elements.html
folder: algorithm
toc: false
---

## Description
Given a collection of **distinct** integers, return all possible permutations.
* 数组里的元素不重复
* 每个元素必须出现且只出现一次
* 元素之间的相对顺序 matters

### Example
* Input: `[1,2,3]`
  * Output:
  ```
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
  ```

## Solution 1: DFS + Swap，速度前 1%
从左到右逐个处理数组里的每一个位置
* 最左边的一个位置有n种可能，用for loop 和 swap来实现
* 左边第二个位置就只有n-1种可能了，当前处于最左边的那一位就不能参与选妃了。也用for loop和swap来实现
* 继续往右搞......

### Complexity
* Time: O(n * n!)
  * 一共有n!种答案。每个答案耗时O(n)
  * 每个答案所需的 n 的时间是消耗在：每个答案得到以后，要用n的时间来造new List来固化这个答案
  * 得到每个答案的时间，其实只需要O(1)，因为我们用的是 swap 方法。具体解释：
    * 比如说要找 1, 2, 3 这三个数一共能组成多少个排列（答案是6种），那么swap方法的解题过程其实是：
      ```
         1         2         3
        / \       / \       / \
       2   3     1   3     1   2
      ```
    * 再往下还有一层，但前两层定了以后，第三层就已经定了
    * 定前两层的时间消耗，可以看出，是 O(6)，与答案的个数是一个量级的！
  * 所以一共有n!种答案，然后n!种答案一共也只需要n!的时间来得到。这也就是 **swap 方法 特别快的原因**
  * 不过在数学上说，n*n! 和 n! 其实是一个量级的，所以这一题的答案也可写为 O(n!)
* Space: O(n)
  * Since we need n call stacks, and each call stack requires constant space

### Java
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        
        if (nums == null || nums.length == 0) {
            return result;
        }
        
        dfsSwap(nums, 0, result);
        
        return result;
    }
    
    private void dfsSwap(int[] nums, int curIndex, List<List<Integer>> result) {
        if (curIndex == nums.length) {
            List<Integer> curPermutation = new ArrayList<>();
            // 写入新的 list 耗时 O(n)
            for (int num : nums) {
                curPermutation.add(num);
            }
            result.add(curPermutation);
            return;
        }
        
        for (int i = curIndex; i < nums.length; i++) {
            swap(nums, curIndex, i);
            dfsSwap(nums, curIndex + 1, result);
            swap(nums, curIndex, i); // 复原
        }
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

## Solution 2: DFS Recursion，速度也是前 1%，但理论上应该会比swap方法慢一点
Ref: http://www.jiuzhang.com/solutions/permutations/

这个方法理论上会比swap方法慢一点的原因在于，swap方法在左边几位固定了之后，只骚扰右边的几位。而这个方法在前几位固定了以后，还是会一遍又一遍地骚扰整个数组里的所有元素，只是因为记录了boolean[] visited 数组，所以不会搞重复。

### Complexity
* Time: O(n * n!)
  * 同理上面的swap方法。一共有n!种答案，每个答案耗时O(n)
* Space: O(n)
  * Since we need n call stacks, and each call stack requires constant space

### Java
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return result;
        }
        
        boolean[] visited = new boolean[nums.length]; // 默认都是false
        dfs(nums, new ArrayList<Integer>(), visited, result);
        return result;
    }
  
    private void dfs(int[] nums, List<Integer> curList, 
                     boolean[] visited, List<List<Integer>> result) {
      
        if (curList.size() == nums.length) {
            // 一定要复制！
            // 因为 curList 在其他分支里还会被反复利用！不是到这里就使命终结了
            result.add(new ArrayList<Integer>(curList));
            return;
        }
              
        // 关键在此。每一次都是试图把整个数组里的每一个数都轮流放进去！
        for (int i = 0; i < nums.length; i++) {
            if (visited[i] == false) {
                curList.add(nums[i]);
                visited[i] = true;
              
                dfs(nums, curList, visited, result);
              
                curList.remove(curList.size() - 1); // 复原
                visited[i] = false; // 复原
            }
        }                       
    }
}
```

## Solution 3: Non-Recursion 的方法，速度也是前 1%
Ref: https://discuss.leetcode.com/topic/6377/my-ac-simple-iterative-java-python-solution

以下为引用：

To permute n numbers, we can add the n-th number into the resulting `List<List<Integer>>` composed by the 
1st, 2nd, 3rd... till the (n-1)th numbers, in every possible position.

For example, if the input array is `{1,2,3}`. 
* Add 1 into the initial List<List<Integer>> (let's call it "answer").
* 2 can be added in front or after 1. So we have to copy the List<Integer> in answer (it's just `{1}`), then add 2 in position 0 of `{1}`; then copy the original `{1}` again, and add 2 in position 1. 
* Now we have an answer of `{{2,1},{1,2}}`. There are 2 lists in the current answer.
* Now we have to add 3. First copy `{2,1}` and `{1,2}`, add 3 in position 0; 
* then copy `{2,1}` and `{1,2}`, and add 3 into position 1; 
* then do the same thing for position 3.
* Finally we have 2 * 3 = 6 lists in answer, which is what we want.

### Complexity
* Time: O(n * n!)
* Space: O(n)

### Java
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
      
        List<List<Integer>> results = new ArrayList<>();
        if (nums == null) {
            return results;
        } else if (nums.length == 0) {
            results.add(new ArrayList<Integer>());
            return results;
        }
        
        int len = nums.length;
        // 把数组里的第一个数放进去，作为后面所有操作的基础
        ArrayList<Integer> firstList = new ArrayList<>();
        firstList.add(nums[0]);
        results.add(firstList);
        
        // 依次把第 2 个到第 n 个数放进去
        for (int i = 1; i < len; i++) {
            int curValue = nums[i];
            List<List<Integer>> tmpResults = new ArrayList<>();
            
            // for each list in the current temp result
            for (List<Integer> curList : results) {
                int curListLen = curList.size();
                
                // for each possible position that we can insert the current value into
                for (int j = 0; j <= curListLen; j++) {
                    List<Integer> tmpList = new ArrayList<>(curList); // 每一次都要new一个！
                    tmpList.add(j, curValue);
                    tmpResults.add(tmpList);
                }
            }
            results = tmpResults;
        }
      
        // 完毕。返回最后的总结果
        return results;
    }
}
```

## Reference
* [Permutations [LeetCode]](https://leetcode.com/problems/permutations/description/)

{% include links.html %}
