---
title: "Cut a String in Every Way to Assemble Another String"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_cut_string_to_assemble_another_string.html
folder: algorithm
toc: false
---

## Description
给了两个String A 和 B。尝试用 B 来组成 A。可以用多个B来组成A。组合之前可以把B的两端分别切掉0到任意个char(s)。
即B只能被削成一个更短的substring或者不削，而不能被切成两个或者更多个substring。
每个B被切的方式都可以不同或相同。
如果无论如何都无法做到组成A，则返回 -1。如果可以组成A，返回最少用多少个B（经历了上面描述的各种切之后的Bs）可以组成A。

### Example
* Input: A = "KAC", B = "JACK"
  * Output: 2。因为B即"JACK"可以用2个来组成A。第一个"JACK"被切为"K"，第二个"JACK"被切为"AC"，那么这两个被切后的substrings就可以组成 "KAC"。所以返回 2

## Solution
我的解法。还需要老师评判是否正确。用DP来做。

首先把A和B的所有可能的substrings包括各个char，都分别放在两个HashSet<String>里。
那么容易想到，如果A里的任何char是B里所没有的，则无论怎么切割和组合B，都不可能得到A，这时就可以直接返回-1了。
 
然后，对于A里的任何一个substring（叫它ss），它最少能被几个B（的被切割后的产物）所组成，等于ss的左substring最少需要几个B，
加上ss的右substring最少需要几个B。ss的左右substring的切分方式需要遍历所有可能。即从ss的左边第一个char的右边分隔开，
直到从ss的右边最后一个char的左边分隔开。

那么就能想到我们需要一个 int[][] dp = new int[n][n]，n = A.length()。它的第一个维度是ss在A里的start index，第二个维度是ss在A里的end index。
那么对于A里的所有的单个char来说，dp[i][i]应该都是等于1的。除非这个char在B里不存在，那么这时直接返回-1，也不用再管这个dp了。

对于fill up 这个二维dp数组，我们就像上一段所述那样，
* 从A的所有长度为1的substring即每个char开始填起，start index要从0到n-1。
* 然后对于A里所有长度为2的substring，填写它们在dp里的相应值。
  * 如果A里这个长度为2的substring在B里也是存在的，那么dp的这一个位置就直接填1
  * 如果不存在，那么：
    ```java
    for (int divider = start + 1; divider <= end; divider++) {
        dp[start][end] = Math.min(dp[start][end], dp[start][divider - 1] + dp[divider][end]);
    }
    ```
* 对于A里所有长度为3、长度为4... 的substrings，依次办理
最终返回 dp[0][n-1]。

### Complexity
这个算法的时间消耗我认为是 O(n^3) + O(m^2)，n是要被拼凑的String A 的长度，m是用来减去字母然后拼凑前者的String B 的长度。

空间消耗我认为是 O(n^2) + O(m^2)，主要是两个Set和一个二维数组的空间消耗。这题没有recursion所以没有多层call stack的空间消耗。
* Time: O(n^3) + O(m^2)
* Space: O(n^2) + O(m^2)

### Java
我的code
```java
class Solution {
    
    public int minComponents(String a, String b) {
        if (a == null || b == null || b.length() == 0) {
            return -1;
        }
        if (a.length() == 0) {
            return 0;
        }
        
        int n = a.length();
        int m = b.length();
        
        // 1st dimension: start index
        // 2nd dimension: end index
        int[][] dp = new int[n][n];
        
        // Time: O(n^2)
        HashSet<String> aSet = populateSet(a);
        // Time: O(m^2)
        HashSet<String> bSet = populateSet(b);
        
        char[] aChars = a.toCharArray();
        // if any char in a is not in b, then we will definitely fail
        for (int i = 0; i < n; i++) {
            String c = Character.toString(aChars[i]);
            if (!bSet.contains(c)) {
                return -1;
            }
            dp[i][i] = 1;
        }
        
        // Time: O(n^3)
        for (int subStrLen = 2; subStrLen <= n; subStrLen++) {
            for (int start = 0; start + subStrLen - 1 < n; start++) {
                int end = start + subStrLen - 1;
                
                if (bSet.contains(a.substring(start, end + 1))) {
                    dp[start][end] = 1;
                    continue;
                }
                
                dp[start][end] = Integer.MAX_VALUE;
                for (int divider = start + 1; divider <= end; divider++) {
                    dp[start][end] = Math.min(dp[start][end], dp[start][divider - 1] + dp[divider][end]);
                }
            }
        }
        
        return dp[0][n - 1];
    }
    
    // Time: O(n^2)
    private HashSet<String> populateSet(String str) {
        HashSet<String> result = new HashSet<>();
        for (int i = 0; i < str.length(); i++) {
            for (int j = i + 1; j <= str.length(); j++) {
                // start index is inclusive, end index is exclusive
                String subStr = str.substring(i, j);
                result.add(subStr);
            }
        }
        return result;
    }
    
    // -------------------------------------------------------------
    // main
    
    public static void main(String[] args) {
        Solution solu = new Solution();
        
        String a = "KAC";
        String b = "JACK";
        
        System.out.println(solu.minComponents(a, b)); // 
    }
}
```

## Reference
* 这是一道Laicode课上题目，尚未发现网上有此题

{% include links.html %}
