---
title: "Longest Common Subarray or Substring"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longest_common_subarray_or_substring.html
folder: algorithm
toc: false
---

## Description
Longest Common Subarray 和 Longest Common Substring 是一样的题目

Given two integer arrays A and B, return the maximum length of an subarray that appears in both arrays.

Note:
* 1 <= len(A), len(B) <= 1000
* 0 <= A[i], B[i] < 100

### Example
* Input: A: [1,2,3,2,1], B: [3,2,1,4,7]
  * Output: 3. The repeated subarray with maximum length is [3, 2, 1].

## Solution: 二维 DP
int dp[i][j]: from array A, ending at index i, from array B, ending at j, the length of their longest repeated subarray

base case: 
* A[i], B[0] if equal, then dp[i][0] = 1
* A[0], B[j] if equal, then dp[0][j] = 1

induction rule:
* if A[i] == B[j], dp[i][j] = dp[i - 1][j - 1] + 1
* else = 0

result: max element in dp matrix

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
public int findLength(int[] A, int[] B) {
	// A and B are guaranteed to be valid arrays with length > 0

	int n = A.length, m = B.length;
	int[][] dp = new int[n][m];

	for (int i = 0; i < n; i++) {
		if (A[i] == B[0]) dp[i][0] = 1;
}
for (int j = 0; j < m; j++) {
	if (B[j] == A[0]) dp[0][j] = 1;
}

int result = 0;
for (int i = 1; i < n; i++) {
	for (int j = 1; j < m; j++) {
		if (A[i] == B[j]) {
dp[i][j] = 1 + dp[i - 1][j - 1];
			result = Math.max(result, dp[i][j]);
		}
}
}
return result;
}
```

## Reference
网上一定有这题，但我还没找

{% include links.html %}
