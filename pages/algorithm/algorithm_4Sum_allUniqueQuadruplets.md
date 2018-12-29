---
title: "Four Sum, Return All Unique Quadruplets (the Order of the 4 Numbers in a Quadruplet Doesn't Matter)"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_4Sum_allUniqueQuadruplets.html                               
folder: algorithm
toc: false
---

## Description
Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target. (返回这些数的值，而非这些数在原数组里的index)

Note:
* The solution set must not contain duplicate quadruplets.
* [1,1,1,2] and [1,1,2,1] are considered as the same quadruplet.
* 数组里可能有重复的元素
* 同一个元素最多只能使用一次。但如果数组里有两个1，则这两个1分别可以被使用一次

### Example
* Input: nums = [1, 0, -1, 0, -2, 2], and target = 0
  * Output: [[-1,  0, 0, 1], [-2, -1, 1, 2], [-2,  0, 0, 2]]

## Solution：见 4 Sum Existence 那题。把4个数化为2个Pair，然后用瞄准一个pair的loop来实现模拟两个pairs
基本的思路见 4 Sum Existence 那一题，那是一种用2个Pair，再加一个HashMap的做法，它耗时 O(n^2)。
本题需要给出所有可能的组合，且不能重复，这个要求比仅仅判断true/false高很多。
给的数组里的数字可能有重复的，而**数组排序以后，很可能有利于去重**，所以我们先把数组排序。

我们需要把 HashMap 从简单的记录 <sum, pair> 改进到记录 <sum, 所有的和为sum的pairs>，这样才能最终得到所有的组合。
注意每个pair记录的是一对elements' indexes，而不是一对elements' values。

**Java 可以把 List<Integer> 放到set或map里做key，Java能正确分辨其异同/重复与否。不能把 int[] 放到set/map里做key，Java无法分辨**

我们用一个长度为2的List来表示一对 elements' indexes，list里的第一个元素就是left element's index，第二个元素就是right element's index。所以总的来说就是：
```
// <pair中2个数的和，和等于前面的key的所有pairs的indexes>
HashMap<Integer, HashSet<List<Integer>>> map;
```

然后，因为数组里的数字有重复，所以我们光保证 "左pair的右index < 右pair的左index" 是不够的！
比如 左pair是 5和5，右pair紧接着左pair，没有重叠，也是5和5，就是说这个数组里至少有4个5，这样就造成重复了，而这样的重复用index是检测不出来的。
但是如果正好target sum 是20，这5个5不就是对的一个组合么 》》》》？？？？？
所以我们还要再搞一个hashset，来存放迄今为止所有遇到过的 pair的值，所谓的值是说它的左右元素的值，而非左右indexes。
注意！HashSet里面可以直接放 List，HashSet对List的equals和hashcode的判断都是ok的！所以再定义一个：
    // 所有出现过的pairs的左右元素的值。左右元素都相等的pairs会被去重，因为这里是一个set
    HashSet<List<Integer>> pairsComponents = new HashSet<>();
至此，我们就可以开始我们的双层 for loop 了。
对于遇到的每一个pair，如果它的值以前出现过（用hashset判断），我们就不管它，看下一个pair。
如果它的值以前没有出现过，之前又存在（一个或几个）pairs与它的和加在一起等于target（用hashmap判断），
那我们把这些pairs都拿出来一个一个看，只要符合 "这个pair的右index < 当前pair的左index"，
则成功凑成一个答案。我们就把这两个pairs构成的 4数组合 加到最终答案里去。
最后，如果当前pair的values以前出现过了（用hashset判断），就是说当前pair是一个重复的pair，那么就什么都不做。
如果当前pair的values以前没有出现过，那就做两件事：
    把当前pair的index加到hashmap里去，这里面还涉及map里是否已经有当前pair的sum的key了，不赘述。
    把当前pair的value加到hashset里去。

### Complexity
* Time: O(n^3) <=== 对么 ？？？？
* Space: O(n), map size <=== 对么 ？？？？

### Java
代码虽然看起来长，其实逻辑不复杂
```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> result = new ArrayList<>();
        HashSet<List<Integer>> dedupResult = new HashSet<>();
        
        if (nums == null || nums.length < 4) {
            return result;
        }
        
        Arrays.sort(nums); // O(nlogn) time
        int n = nums.length;
        // <sum of pair, indexes of pairs that sum up to the key>
        Map<Integer, Set<List<Integer>>> map = new HashMap<>();
        
        for (int i = 0; i < n - 1; i++) { // left index of pair, O(n) time
            for (int j = i + 1; j < n; j++) { // right index of pair, O(n) time
                int sum = nums[i] + nums[j];
                List<Integer> curPair = makePair(i, j);
                
                // case 1: pairs of the same sum as the current pair
                Set<List<Integer>> pairsOfSameSum = map.get(sum);
                
                if (pairsOfSameSum == null) {
                    Set<List<Integer>> set = new HashSet<>();
                    set.add(curPair);
                    map.put(sum, set);
                } 
                else if (sum * 2 != target) {
                    pairsOfSameSum.add(curPair);
                } 
                else { // sum * 2 == target
                    for (List<Integer> pair : pairsOfSameSum) { // O(n) time
                        // the right index of the previous pair must be smaller than
                        // the left index of the current pair
                        if (pair.get(1) < curPair.get(0)) {
                            dedupResult.add(makeQuadruplet(nums, pair, curPair));
                        }
                    }
                    pairsOfSameSum.add(curPair);
                    continue;
                }
                
                // case 2: pairs of the complement sum to the current pair
                Set<List<Integer>> pairsOfComplementSum = map.get(target - sum);
                
                if (pairsOfComplementSum != null) {
                    for (List<Integer> pair : pairsOfComplementSum) {
                        if (pair.get(1) < curPair.get(0)) {
                            dedupResult.add(makeQuadruplet(nums, pair, curPair));
                        }
                    }
                }
            }
        }
        
        for (List<Integer> quadru : dedupResult) {
            result.add(quadru);
        }
        return result;
    }
    
    // helper function
    private List<Integer> makePair(int a, int b) {
        List<Integer> pair = new ArrayList<>();
        pair.add(a);
        pair.add(b);
        return pair;
    }
    
    // helper function
    private List<Integer> makeQuadruplet(int[] nums, List<Integer> l1, List<Integer> l2) {
        List<Integer> quadru = new ArrayList<>();
        for (int index : l1) {
            quadru.add(nums[index]);
        }
        for (int index : l2) {
            quadru.add(nums[index]);
        }
        return quadru;
    }
}
```

## Reference
[4Sum [LeetCode]](https://leetcode.com/problems/4sum/description/)

{% include links.html %}
