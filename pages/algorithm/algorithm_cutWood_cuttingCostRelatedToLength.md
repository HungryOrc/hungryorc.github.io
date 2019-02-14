---
title: "Cut Wood: Cutting Cost Related to Length"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_cutWood_cuttingCostRelatedToLength.html
folder: algorithm
toc: false
---

## Description
There is a wooden stick with length L >= 1, we need to cut it into pieces, where the cutting positions are defined in an int array A. 
The positions are guaranteed to be in ascending order in the range of [1, L - 1]. 
The cost of each cut is the length of the stick segment being cut. 
Determine the minimum total cost to cut the stick into the defined pieces.

在一个木头上的任何位置切一刀，这一刀的cost都和本段木头在 **切割前** 的长度成正比。
所以，按不同的顺序切开一个木头，最后的累计总cost是不同的。
然后这一题要求在所有标记的长度上最终都必须切开。

如果一个木头长为10，在2，4，7处都做了标记，那么就是要求必须在2米，4米，7米这三处都切开。
如果第一刀切2米处，则这一刀的cost为10；
然后第二刀切原先的4米处的话，这一刀的cost为8，因为10-2=8；
第三道切原先的7米处，这一刀的cost为6，因为切完第二刀之后，7米处所在的那一截的长度为6。
那么总cost就是 10+8+6=24。

### Example
* Input: L = 10, A = {2, 4, 7}
  * Output: the minimum total cost is 10 + 4 + 6 = 20 (cut at 4 first then cut at 2 and cut at 7)

## Solution：2维DP
思路：我们把题目给的数组做一个扩展，A = {2, 4, 7} --> {0, 2, 4, 7, 10}，即把开头和结尾的长度都包含进来了.

* DP数组 `M[i][j]` 表示 the min cost of cutting **EVERY POSITION** the wood between index i and index j in the input array A.
So for this example, the sulution we are looking for is `M[0][4]`

* Base Case: The adjacent cutting positions that cannot be cut any further: 
  ```
  M[0][1] = 0, M[1][2] = 0, M[2][3] = 0... M[i-1][i] = 0
  ```

* Induction Rules
  * Size = 1: Adjacent indexes: [left = i, right = i + 1]
    ```
    M[0][1] = M[1][2] = M[2][3] = M[3][4] = 0
    ```
  * Size = 2: [left = i, right = i + 2]
    ```
    M[0][2] = M[0][1] + M[1][2] + (cutting cost of the wood from index 0 to index 2)
            = M[0][1] + M[1][2] + (length from index 0 to index 2) 
            = M[0][1] + M[1][2] + (A[2] - A[0]) = 0 + 0 + (4 - 0) = 4
    M[1][3] = M[1][2] + M[2][3] + (A[3] - A[1]) = 0 + 0 + (7 - 2) = 5
    M[2][4] = M[2][3] + M[3][4] + (A[4] - A[2]) = 0 + 0 + (10 - 4) = 6
    ```
  * Size = 3: [left = i, right = i + 3]
    ```
    M[0][3]
        cutting cost of the wood from index 0 to index 3 = A[3] - A[0] = 7 - 0 = 7
        Case 1: first cut between index 0 and index 3 is at index 1
            M[0][3] = M[0][1] + M[1][3] + (A[3] - A[0]) = 0 + 5 + 7 = 12
        Case 2: first cut between index 0 and index 3 is at index 2
            M[0][3] = M[0][2] + M[2][3] + (A[3] - A[0]) = 4 + 0 + 7 = 11
        So, M[0][3] = min(Case1, Case2) = 11
    ```
    ```
    M[1][4]
        cutting cost of the wood from index 1 to index 4 = A[4] - A[1] = 10 - 2 = 8
        Case 1: first cut between index 1 and index 4 is at index 2
            M[1][4] = M[1][2] + M[2][4] + (A[4] - A[1]) = 0 + 6 + 8 = 14
        Case 2: first cut between index 1 and index 4 is at index 3
            M[1][4] = M[1][3] + M[3][4] + (A[4] - A[1]) = 5 + 0 + 8 = 13
        So, M[1][4] = min(Case1, Case2) = 13
    ```
  * Size = 4: [left = i, right = i + 4]
    ```
    ...
    ...
    ```

* 由此，我们把M[i][j]这个DP矩阵画出来，如下。其中所有标为`N/A`的cell，都是没有意义的cell，我们不会涉及。
  ```
  N/A   0    4    x   x
  N/A  N/A   0    5   x
  N/A  N/A  N/A   0   6
  N/A  N/A  N/A  N/A  0
  N/A  N/A  N/A  N/A N/A
  ```

* 所以：**关键结论**
  * DP matrix 里的每个数，是根据他的下方和左方的多个数，或者说多对数所决定的！即 M[i][j] 是由 M[i][j-1]与M[j-1][j]、M[i][j-2]与M[j-2][j]... M[i][i+1]与M[i+1][j] 这一对对数决定的
  * 这几对数，再加上cutting cost，得到几个可能值，再在这几个可能值里面取min，得到 M[i][j]

### Complexity
* Time: O(n^2 * n) = O(n^3)，最后这个n，意思就是，每确定一个M[i][j]，最多需要O(n)对数来计算确定，最终取min得M[i][j]
* Space: O(n^2)

### Java
我的代码实现。这题思路不简单，代码反而较短。DP题往往代码不长，就算这个题目本身比较难。
```java
public class Solution {
    public int minCost(int[] cuts, int length) {
        // make an expanded array
        int[] endpoints = new int[cuts.length + 2];
        int n = endpoints.length;
        endpoints[0] = 0;
        endpoints[n - 1] = length;
        
        for (int i = 1; i < n - 1; i++) {
            endpoints[i] = cuts[i - 1];
        }
    
        // DP Matrix
        int[][] M = new int[n][n];
    
        // 边界条件，this is when the size of the wood sections are 1
        // 这里就是填入斜对角线的一串0
        for (int i = 0; i < n - 1; i++) {
            M[i][i + 1] = 0;
        }
    
        // 填入右上方的其他cells的值
        for (int end = 2; end < n; end++) {
        
            // 这个写法比较特殊，i和j都++，相当于是沿着对角线方向向右下角滑动
            // 这题还有一种更直观的写法，就是外层循环用len=2,3,4... 我回头自己试一下，test一下
            for(int j = end, i = 0; j < n; i++, j ++) {
        
                M[i][j] = Integer.MAX_VALUE;
                
                // 初始值
                int cuttingCost = endpoints[j] - endpoints[i];
        
                for (int mid = i + 1; mid <= j - 1; mid++) {
                    int curCost = M[i][mid] + M[mid][j] + cuttingCost;
                    M[i][j] = Math.min(curCost, M[i][j]);
                }
            }
        }

        return M[0][n - 1];
    }
}
```

## Reference
* [应该有这题，只是我没找 [LeetCode]](网址放在这里)

{% include links.html %}
