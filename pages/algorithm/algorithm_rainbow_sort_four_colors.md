---
title: "Rainbow Sort - Four Colors"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rainbow_sort_four_colors.html
folder: algorithm
toc: false
---

## Description
Given an array of balls, where the color of the balls can only be Red, Green, Blue or Black, sort the balls such that all balls with same color are grouped together and from left to right the order is Red->Green->Blue->Black. (Red is denoted by 0, Green is denoted by 1,  Blue is denoted by 2 and Black is denoted by 3).

### Example
* Input: {1, 3, 1, 2, 0}
  * Output: {0, 1, 1, 2, 3}

## Solution
四个隔板，两个从左往右，两个从右往左。
* 第一个隔板的左边不含自己都是0
* 第二个隔板的左边不含自己，也不不含第一个隔板的左边，都是1
* 第四个隔板的右边不含自己都是3
* 第三个隔板的右边不含自己，也不含第四个隔板的右边，都是2
* [第二个隔板, 第三个隔板]这个双头闭区间之内 都是未知的值

第二个隔板，其实是扮演了“*当前所考察的位置*”！

特别注意！！把第四个隔板向左移的时候，不仅要同时向左移第三个隔板，而且还要swap两次！第一次是把隔板二指向的数与隔板三指向的数swap，然后是把隔板三和隔板四所指向的数swap。

while loop的判断条件是第二个隔板<=第三个隔板。这样同时保证了隔板二不超右边，也保证了隔板三不超左边。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
  public int[] rainbowSortII(int[] array) {
    if (array == null || array.length <= 1) {
      return array;
    }
    
    int len = array.length;
    int rd = 0, gr = 0;
    int bu = len - 1, bk = len - 1;
    
    while (gr <= bu) {
      if (array[gr] == 0) {
        swap(array, rd, gr);
        rd++;
        gr++;
      } else if (array[gr] == 1) {
        gr++;
      } else if (array[gr] == 2) {
        swap(array, gr, bu);
        bu--;
      } else { // array[gr] == 3
        swap(array, gr, bu); // 特别注意！这里要swap两次！！
        swap(array, bu, bk);
        bk--;
        bu--;
      }
    }
    return array;
  }
  
  private void swap(int[] array, int i, int j) {
    int tmp = array[i];
    array[i] = array[j];
    array[j] = tmp;
  }
}
```

## Reference
* [Rainbow Sort II [LaiCode]](https://app.laicode.io/app/problem/399)

{% include links.html %}
