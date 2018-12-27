---
title: "Two Sum: Data Structure Design"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_2Sum_data_structure_design.html
folder: algorithm
toc: false
---

## Description
Design and implement a TwoSum class. It should support the following operations: `add` and `find`.
* add - Add the number to an internal data structure.
* find - Find if there exists any pair of numbers which sum is equal to the value.

Your TwoSum object will be instantiated and called as such:
```java
TwoSum obj = new TwoSum();
obj.add(number);
boolean param_2 = obj.find(value);
```

### Example
* Input: add(1); add(5); add(3);
  * Output: find(4) -> true; find(7) -> false

## Solution
这一题要看实际操作里，是add多还是find多。如果add多，就把复杂的计算放到find里，反之亦然。从leetcode给的test case看，是add多。

### Complexity
* Time:
  * add: O(1)
  * find: O(n)
* Space: O(n), map

### Java
```java
class TwoSum {
    Map<Integer, Integer> nums;

    public TwoSum() {
        nums = new HashMap<>();
    }
    
    /** Add the number to an internal data structure.. */
    public void add(int number) {
        nums.put(number, nums.getOrDefault(number, 0) + 1);
    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        for (Map.Entry<Integer, Integer> entry : nums.entrySet()) {
            int num = entry.getKey();
            int occurance = entry.getValue();
            
            if (num * 2 == value && occurance >= 2) {
                return true;
            }
            
            // 下面这个 num * 2 != value 是为了对付一种阴险的情况：
            // target value = 0，然后之前只放入过一个 0. 如果没有这个判断，则
            // nums.containsKey(value - num)会通过，即会返回true，但其实我们只有1个0，无法
            // 完成2个0才能完成的 0 + 0 = 0
            if (num * 2 != value && nums.containsKey(value - num)) {
                return true;
            }
        }
        return false;
    }
}
```

## Reference
* [Two Sum III - Data structure design [LeetCode]](https://leetcode.com/problems/two-sum-iii-data-structure-design/description/)

{% include links.html %}
