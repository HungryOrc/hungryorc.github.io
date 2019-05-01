---
title: "Degree of Array"
tags: [algorithm, subarray]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_degree_of_array.html
folder: algorithm
toc: false
---

## Description
一个数组的"度(Degree)"等于它里面出现最多的元素的次数。非空数组nums的元素都是非负整数。
如果整个数组 nums 的 degree 为n，找到degree 也为 n 的连续 subarray 的最小长度。

### Example
* Input: [1, 2, 2, 3, 1]
  * Output: 2
  * nums的degree为2，因为元素1和2都出现了2次。和原数组具有相同度数的连续子数组有：[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]。其中最短的长度为2。
* Input: [1,2,2,3,1,4,2]
  * Output: 6

## Solution
目标子数组的首元素一定是 nums 中出现最多的元素（或之一，设它为 a）在整个数组里的第一次出现。
目标子数组的尾元素一定是同样的这个 a 在整个数组里的最后一次出现。
所以问题转化为：求nums中出现次数最多的某个元素第一次出现，到最后一次出现的子数组的最小长度。

所以只需 walk through 数组 nums 一次，在此过程中用 2 个 map：一个记录当前元素值到目前为止出现的次数；另一个记录当前元素值出现的首位置，用于计算子数组长度。过程中要不断判断：
* 是否需要更新 所有元素值出现的最大次数 maxCount
* 如果当前元素值的出现次数 curCount == maxCount，可能需要更新 最短子数组长度 minLen。如果 curCount > maxCount，则必须更新 minLen。

### Attention
* We are setting `minLen = nums.length` initially, but if all the numbers in the array only appear once, then we must return `minLen` to be 1! If any of the numbers in the array appears more than once, then `minLen` can be calculated correctly by the following algorithm.

### Complexity
* Time: O(n)
* Space: O(n)

### Java

```java
class Solution {
    
    public int findShortestSubArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        HashMap<Integer, Integer> startIndexes = new HashMap<>();
        HashMap<Integer, Integer> counts = new HashMap<>();
        
        int arrayLen = nums.length;
        int minLen = arrayLen;
        int curLen = 0;
        int maxCount = 0;
        
        for (int i = 0; i < arrayLen; i++) {
            int curNum = nums[i];
            int curCount = counts.getOrDefault(curNum, 0);
            
            if (curCount == 0) {
                counts.put(curNum, 1);
                startIndexes.put(curNum, i);
                curLen = 1;
            } else {
                curCount ++;
                counts.put(curNum, curCount);
                int curStart = startIndexes.get(curNum);
                curLen = i - curStart + 1;
            }
            
            if (curCount > maxCount) {
                minLen = curLen;
                maxCount = curCount;
            } else if (curCount == maxCount) {
                if (curLen < minLen) {
                    minLen = curLen;
                }
            }
        }

        return minLen;
    }
}
```

### C++

```c++
class Solution {
public:
    int degreeOfArray(vector<int>& nums) {
        unordered_map<int, int> startIndex, count; // 2 Hash Maps
        int minLen = INT_MAX, maxCount = 0;

        for (int i = 0; i < nums.size(); i++) {
            int ni = nums[i];
            if (startIndex.find(ni) == startIndex.end()) { // == unordered_map.end() 表示不存在，即iterate到尾部还是没能找到
                startIndex[ni] = i;
            }

            int curLen = i - startIndex[ni] + 1;
            int curCount = ++count[ni];

            if (curCount == maxCount) {
                minLen = min(minLen, curLen);
            } else if (curCount > maxCount) {
                minLen = curLen;
                maxCount = curCount;
            }
        }
        return minLen;
    }
};
```


## Reference
* [Degree of an Array - LeetCode](https://leetcode.com/problems/degree-of-an-array/description/)
* [Answer for Degree of an Array - 九章算法](https://www.jiuzhang.com/solution/shu-zu-de-du-shu/)

{% include links.html %}
