---
title: "Min Transformed Values on an Array"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_minTransformedValues_onArray.html
folder: algorithm
toc: false
---

## Description
把一个int数组转化成一个transformed array，这个新的array里的每个数都必须是正整数，且能体现原数组里的数的大小关系，且新数组里的最大值要尽可能地小

比如给一个数组 `{10, 70, 50, -30, 0}`，得到的结果应该是 `{3, 5, 4, 1, 2}`

### Example
如上

## Solution
用一个map即可。要注意：原数组里可能有重复的值！

### Complexity
* Time: O(n)
* Space: O(n)，map size

### Java
```java
class Solution {
    public int[] transformArray(int[] nums) {
        int n = nums.length; 
        
        int[] sortedNums = new int[n];
        for (int i = 0; i < n; i++) {
            sortedNums[i] = nums[i];
        }

        Arrays.sort(sortedNums);
        
        // <value, index in the sorted array>
        Map<Integer, Integer> map = new HashMap<>();
        map.put(sortedNums[0], 1);
        int count = 1;
        for (int i = 1; i < n; i++) {
            if (sortedNums[i] > sortedNums[i - 1]) {
                count++;
                map.put(sortedNums[i], count);
            }
        }
        
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = map.get(nums[i]);
        }
        return result;
    }
}
```

## Reference
* 我自己被Waymo电面的题目，没在网上看到这题

{% include links.html %}
