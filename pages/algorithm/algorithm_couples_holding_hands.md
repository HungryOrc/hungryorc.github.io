---
title: "Couples Holding Hands"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_couples_holding_hands.html
folder: algorithm
toc: false
---

## Description：这题的难度是Hard，但其实不艰深，只是思路很巧妙 不易想到
N couples sit in 2N seats arranged in a row and want to hold hands. We want to know the minimum number of swaps so that every couple is sitting side by side. A swap consists of choosing any two people, then they stand up and switch seats.

The people and seats are represented by an integer from 0 to 2N-1, the couples are numbered in order, the first couple being (0, 1), the second couple being (2, 3), and so on with the last couple being (2N-2, 2N-1).

The couples' initial seating is given by row[i] being the value of the person who is initially sitting in the i-th seat.

Notes:
* len(row) is even and in the range of [4, 60].
* row is guaranteed to be a permutation of 0...len(row)-1.

注意：
* 在最后的结果里，每一个pair可以是比如 `{2, 3}`，也可以是 `{3, 2}`
* 在最后的结果里，pair与pair之间的顺序完全无所谓。但既然我们要用最少的swap次数，那么我们就要尽量多地利用原数组里现有的elements。更进一步说，就是要原数组里的处在 index 为 0、2、4、6..... 的数都不动，奇数位的elements做出调整以与它之前紧贴的偶数位的element组成配对

### Example
* Input: row = [0, 2, 1, 3]
  * Output: 1. We only need to swap the second (row[1]) and third (row[2]) person.
* Input: row = [3, 2, 0, 1]
  * Output: 0. All couples are already seated side by side.

## Solution：思路很巧妙：用一个数组（map也行）来记录每个数字i在原数组里处于哪个index处。速度 前1%
比如一开始数组是 `{0, 2, 4, 6, 7, 1, 3, 5}`，
* 那么第一步，我们要把0后面的2 与 1 swap，得到：`{0, 1, 4, 6, 7, 2, 3, 5}`
* 然后看第三位的4，它的炮友是5（注意每一个pair里都必然是偶数小奇数大的模式），所以我们要把4后面的6 与5 swap，得到：`{0, 1, 4, 5, 7, 2, 3, 6}`
* 以此类推。可以想到，处理后面的时候，绝不会影像前面

### Complexity
* Time: O(n)
* Space: O(n)

### Java
思路挺简明的。
```java
class Solution {
    public int minSwapsCouples(int[] row) {
        if (row == null || row.length == 0 || row.length % 2 == 1) {
            return Integer.MAX_VALUE;
        }
        
        int n = row.length;
        
        // 对于一个数字i来说，它处于原始数组的什么index上
        // 比如原始数组是 {0, 3, 1, 2} 的话，它的indexes数组就是 {0, 2, 3, 1}
        int[] indexes = new int[n];
        for (int i = 0; i < n; i++) {
            int value = row[i];
            indexes[value] = i;
        }
        
        int count = 0;
        
        // 进行鬼斧神工的swaps
        for (int i = 0; i < n; i += 2) {
            // 每个pair我们设为left和right两个数
            int left = row[i];
            
            int right = -1;
            if (left % 2 == 0) {
                right = left + 1;
            } else {
                right = left - 1;
            }
            
            if (row[i + 1] == right) continue; // 正好匹配了，不必swap了
            
            int indexOfRightInOriginalArray = indexes[right];
            
            // 注意！别忘了更新在 i + 1 这个位置上的 value 在indexes array 里的 index！
            // 因为这个数不再待在 i + 1 这个位置了！它被swap到 indexOfRightInOriginalArray 这个位置了！
            // 而且要在swap之前做这事！因为swap是要改变row array 的！
            // 这是本题的关键点之一
            indexes[row[i + 1]] = indexOfRightInOriginalArray;
            
            swap(row, i + 1, indexOfRightInOriginalArray);
            count++;
        }
        return count;
    }
    
    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

## Reference
* [Couples Holding Hands [LeetCode]](https://leetcode.com/problems/couples-holding-hands/description/)

{% include links.html %}
