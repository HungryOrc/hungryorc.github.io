---
title: "Form one Triangle"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_formTriangle_oneTriangle.html
folder: algorithm
toc: false
---

## Description
给一个int数组，里面都是正整数，问能否找到任意三个数，组成一个三角形

### Example
略 

## Solution
* 能组成三角形的充要条件：**最短的两个边的和 >= 最大的那个边**  
* 于是方法：把数组从小到大排列，考察邻近的每三条边，任何一个三边组合成功，就可返回true。
  * 注意：如果任何三条边能组成三角形，那么往它们三个的中间找，相邻的三个边也一定能组成三角形，因为三边之间的间隔变小了，那么两个短边加在一起 >= 最长边的可能性也就增加了

### Complexity
* Time: O(n logn)，排序需要时间
* Space: O(1)

### Java
```java
public class Solution {
    public boolean canFormTriangle(int[] nums) {
        if (nums == null || nums.length < 3) {
            return false;
        }
        
        int n = nums.length;
        Arrays.sort(nums);
        
        for (int i = 2; i < n; i++) {
            if (nums[i - 2] + nums[i - 1] >= nums[i]) {
                return true;
            }
        }
        return false;
    }
}
```

## Reference
* 网上没找到这题

{% include links.html %}
