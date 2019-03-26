---
title: "Form one Triangle"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_formTriangle_numOfTriangles.html
folder: algorithm
toc: false
---

## Description
给一个int数组，里面都是正整数，问能找到多少组三个数，每一组能组成一个三角形。

数组里可能有重复的数。

### Example
略 

## Solution：排序，然后双指针，一前一后相向而行
* 能组成三角形的充要条件：**最短的两个边的和 >= 最大的那个边**  
* 于是方法：把数组从小到大排列，每次固定一个最大的边（可认为是第三边）。那么前两边的和要小于它。而且前两边的index要递减。
* 这就成了一个 在int array（sub-array）上找有多少对元素的和 <= target（即三元组里最右边的最大的那个数）的问题！
  * 所以，双指针，一前一后相向而行

### Complexity
* Time: O(nlogn + n^2) = O(n^2)
  * 其中nlogn是排序
  * 后面的n^2是：对于每个target都需要n的时间来完成双指针的运动，一共有n个target
* Space: O(1)

### Java
```java
public class Solution {
    public boolean numOfTriangles(int[] nums) {
        if (nums == null || nums.length < 3) {
            return 0;
        }
        
        int n = nums.length;
        Arrays.sort(nums);
        int count = 0;
        
        for (int i = n - 1; i >= 0; i--) { // 三元组里最右边的最大的那个数
            int target = nums[i];
            
            int r = i - 1, l = 0;
            
            for (; r > l; r--) {
                while (nums[l] + nums[r] <= target) {
                    l++;
                }
                count += (r - l);
            }
        }
        return count;
    }
}
```

## Reference
* 网上没找到这题

{% include links.html %}
