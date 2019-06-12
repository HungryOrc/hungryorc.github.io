---
title: "Distinct Subsequences II"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_distinct_subsequences_ii.html
folder: algorithm
toc: false
---

## Description
Given a string S, count the number of distinct, non-empty subsequences of S .

Since the result may be large, return the answer modulo 10^9 + 7.

Note:
* S contains only lowercase letters.
* 1 <= S.length <= 2000

### Example
* Input: "abc"
  * Output: 7. The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".
* Input: "aba"
  * Output: 6. The 6 distinct subsequences are "a", "b", "ab", "ba", "aa" and "aba".

## Solution
思路并不好想。答案的思路在此：https://leetcode.com/problems/distinct-subsequences-ii/solution/

每个sub sequence可以看成一个string。那么 a list of 不重复的 sub sequence 就是一些不重复的strings。现在要往这些strings的尾部同时加上一个字母。
比如说x。那么我们要找的是，这些strings里面，有哪些string的尾部加了一个x以后会等于另一个已经存在于这些strings里的string。

那么我们设当前是第i步。再设我们在之前的第j步的时候进行了最近的一次的给所有strings的尾部添加x的行动。那么我们再思考第j-1 步的时候。这时候list of strings里同样是不允许有重复的string的。我们设想在j-1 步的时候，共有n个不重复的strings。那么到了第j步的时候，这n个strings还是符合要求的答案的一部分，设为list a（自然它的长度是n）；j步的答案的另一部分是这n个strings里尾部都加了一个x以后得到的一些strings，进行了与list a 的去重以后，剩下的list设为 list b（所以自然list b 里的每个string的结尾都一定是x）。j步的答案就是list a 和 list b 的concatenate。

那么到了第i步的时候，与第j步相比，会多出来一些strings，但我们确定的是，list a 和 list b 里的每一个string一定都在第i步的答案里一个不少的出现，因为他们都是符合要求的string。那么在第i步里，在对它们每一个的尾部加上一个x，情况将会是：
* 对于list a 里的每一个string的尾部加一个x都会制造出一个已经存在的string，这个存在可能是与list a 里的另一个string重复（比如 list a 是 {a, ax, bc} 的话），也可能与list b里的某一个string 重复。但总之list a 的每个string后面加x所得到的strings都不能要，全都一定是重复的
* 对于list b 里的每一个string，它们的尾部加上一个x以后都一定不会和任何现有的strings相同，因为：
  * 首先这样加一个x得到的strings不可能和list a 里的任何string相同，因为list b 里的每一个string都一定比list a 里的每一个string要长
  * 然后，这些得到的strings也不可能和list b 里的任何string相同。就是说list b 里不可能有任何sting是在尾部加了一个x之后能等于list b 里的任何一个别的string。因为如果可以，比如说list b 里有一个 abx，还有一个 abxx，那么list a 里就一定有 ab 和 abx，那么list a 里的abx和list b 里的abx就重复了。而按照题意，list a和b在任何时候都是不可能有任何元素重复的。
* 综上所述，我们才能有这个题的至尊无敌公式：
```java
// assume s.charAt(i) == 'x'
// and `int lastX` means the index of the latest occurence position of char x in string s 
dp[i] = 2 * dp[i - 1] - (dp[lastX - 1]);
```

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    private static final int MOD = 1_000_000_007;
    
    public int distinctSubseqII(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int n = s.length();
        int[] dp = new int[n]; // 从index=0到index=i这一段（未必在i处结尾）共有多少种不同的sub sequences
        Map<Character, Integer> lastOccurIndex = new HashMap<>();
        
        dp[0] = 2; // 什么都没有，以及放一个首字母，这两种
        lastOccurIndex.put(s.charAt(0), 0);
        
        for (int i = 1; i < n; i++) {
            char c = s.charAt(i);
            
            Integer lastOccurIndexOfC = lastOccurIndex.get(c);
            int duplicateCount = 0;
            if (lastOccurIndexOfC == null) { // c 之前没出现过
                duplicateCount = 0;
            } else if (lastOccurIndexOfC == 0) { // c 上一次出现是在s的头部第一个字母处
                duplicateCount = 1; // 这个1是指什么都没有的时候的那个空sub sequence。或者说是用 2 * 2 - 3 = 1 反推得来的
            } else {
                duplicateCount = dp[lastOccurIndexOfC - 1];
            }
            
            dp[i] = (2 * dp[i - 1]) % MOD - (duplicateCount) % MOD;
            
            // 特别注意！有可能duplicateCount然后就把dp[i]给减成负数了！
            if (dp[i] <= 0) dp[i] += MOD;
            
            lastOccurIndex.put(c, i);
        }
        
        //if (dp[n - 1] < MOD)  return dp[n - 1] + MOD - 1;
        return dp[n - 1] % MOD - 1;
    }
} 
```

## Reference
* [Distinct Subsequences II [LeetCode]](https://leetcode.com/problems/distinct-subsequences-ii/description/)

{% include links.html %}
