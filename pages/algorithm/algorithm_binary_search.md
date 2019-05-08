---
title: "Binary Search"
tags: [algorithm, algorithm_overview]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_binary_search.html
folder: algorithm
toc: false
---

## Overview

### Complexity
* Time: 平均情况下 O(logn), worst case O(n^2)
* Space: 平均情况下 O(1)

## Implementation

### Java

#### 九章版
Ref: http://www.jiuzhang.com/solutions/binary-search/
```java
public int findPosition(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return -1;
    }  
  
    int start = 0, end = nums.length - 1;
    // 注意 while 条件的选择，用了一个 +1，这是 九章特色
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            start = mid; // 注意！这里不 mid+1
        } else {
            end = mid; // 注意！这里不 mid-1
        }
    }
    
    // 此时 start + 1 = end。二者都可能是target，只能都比较下
    if (nums[start] == target) {
        return start;
    } else if (nums[end] == target) {
        return end;
    }
    return -1;
}
```

#### 经典版
```java
public int binarySearch(int[] A, int target) {
    if (A == null || A.length == 0) {
        return -1;
    }

    int left = 0, right = A.length - 1;
    // 注意，这里用 left <= right，不要用 left < right
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (A[mid] > target) {
            right = mid - 1;
        } else if (A[mid] < target) {
            left = mid + 1;
        } else { // A[mid] == target
            return mid; // 找到了target，就直接return
        }
    }

    // 到了这里意味着没有找到target
    // 此时 right + 1 = left
    return -1;
}
```

{% include links.html %}
