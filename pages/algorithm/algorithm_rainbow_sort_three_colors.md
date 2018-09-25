---
title: "Rainbow Sort - Three Colors"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rainbow_sort_three_colors.html
folder: algorithm
toc: false
---

## Description
Given an array of balls, where the color of the balls can only be Red, Green or Blue, sort the balls such that all the Red balls are grouped on the left side, all the Green balls are grouped in the middle and all the Blue balls are grouped on the right side. (Red is denoted by -1, Green is denoted by 0, and Blue is denoted by 1).

### Example
* Input: {1, 0, 1, -1, 0}
  * Output: {-1, 0, 0, 1, 1}

## Solution
三个隔板，两个从左往右，一个从右往左。
* 第一个隔板的左边不含自己都是-1
* 第二个隔板的左边不含自己，也不不含第一个隔板的左边，都是0
* 第三个隔板的右边不含自己都是1
* [第二个隔板, 第三个隔板]这个双头闭区间之内 都是未知的值

第二个隔板，其实是扮演了“*当前所考察的位置*”！

while loop的判断条件是第二个隔板<=第三个隔板。这样同时保证了隔板二不超右边，也保证了隔板三不超左边。
* 比如输入数组是{1}的话，前两个隔板不会动，第三个隔板会-1，即由0变成-1，然后还不退出while loop的话，下一个loop里就向左越界了。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
  public int[] rainbowSort(int[] array) {
    if (array == null || array.length == 0) {
      return array;
    }
    
    int len = array.length;
    int rd = 0; // rd左边不含rd 都是-1
    int gr = 0; // gr左边不含gr，除了rd左边，都是0
    int bl = len - 1; // bl右边不含bl 都是1
    
    // green index 其实也扮演了 cur index 的角色
    while (gr <= bl) {
      if (array[gr] == -1) {
        swap(array, rd, gr);
        rd++;
        gr++;
      } else if (array[gr] == 0) {
        gr++;
      } else { // array[i] == 1
        swap(array, bl, gr);
        bl--;
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
* [Rainbow Sort [LaiCode]](https://app.laicode.io/app/problem/11)

{% include links.html %}
