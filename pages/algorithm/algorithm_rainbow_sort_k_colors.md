---
title: "Rainbow Sort - K Colors"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rainbow_sort_k_colors.html
folder: algorithm
toc: false
---

## Description
Given an array of balls with k different colors denoted by numbers 1 to k, sort the balls.

### Example
* Input: {3, 1, 5, 5, 1, 4, 2}, k = 5
  * Output: {1, 1, 2, 3, 4, 5, 5}

## Solution 1
类似二分法的做法。根据最小color和最大color算出middle color，然后用middle color把整个数组粗排一遍。
然后再对前后半段分别排，相应的color range也要缩小。

### Complexity
* Time: O(n logk)
* Space: O(1), in place

### Java
```java
public class Solution {
    
    public int[] rainbowSortIII(int[] array, int k) {
      if (array == null || array.length == 0) {
        return array;
      }
      if (k <= 0) {
        return array;
      }
      
      int len = array.length;
      int startColor = 1, endColor = k;
      int left = 0, right = len - 1;
      
      sortKColors(array, left, right, startColor, endColor);
      return array;
    }
    
    private void sortKColors(int[] array, int left, int right,
                            int startColor, int endColor) {
      if (left >= right) {
        return;
      }
      if (startColor >= endColor) {
        return;
      }
      
      int l = left, r = right;
      int midColor = startColor + (endColor - startColor) / 2;
      
      while (l <= r) {
        while (l <= r && array[l] <= midColor) {
          l++;
        }
        while (l <= r && array[r] > midColor) {
          r--;
        }
        if (l <= r) {
          swap(array, l++, r--);
        }
      }
      
      sortKColors(array, left, r, startColor, midColor);
      sortKColors(array, l, right, midColor + 1, endColor);
    }
    
    private void swap(int array[], int i, int j) {
      int tmp = array[i];
      array[i] = array[j];
      array[j] = tmp;
    }
}
```

## Solution 2
从左到右走两遍。第一遍把每种颜色的个数记下。第二遍依次涂色。
这个看起来过于直接，有点傻，其实速度比上一个看似高明的方法还要快，达到了 O(n+k)。
小缺点就是要一点额外空间 O(k)。

### Complexity
* Time: O(n+k)
* Space: O(k)

### Java
```java
public class Solution {
    
    public int[] rainbowSortIII(int[] array, int k) {
      if (array == null || array.length == 0) {
        return array;
      }
      if (k <= 0) {
        return array;
      }
      
      int len = array.length;
      int[] counts = new int[k + 1]; // 0 - k, not using 0
      
      // Time: O(n)
      for (int i = 0; i < len; i++) {
        int color = array[i];
        counts[color]++;
      }
      
      // Time: O(k + n)
      int start = 0, end = 0;
      for (int color = 1; color <= k; color++) {
        end = start + counts[color] - 1;
        for (int j = start; j <= end; j++) {
          array[j] = color;
        }
        start = end + 1;
      }
      
      return array;
    }
}
```

## Reference
* [Rainbow Sort III [LaiCode]](https://app.laicode.io/app/problem/400)

{% include links.html %}
