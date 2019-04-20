---
title: "Evaluate Division"
tags: [algorithm, union_find]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_evaluate_division.html
folder: algorithm
toc: false
---

## Description
Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

Example:
* Given a / b = 2.0, b / c = 3.0. 
* queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? . 
* return [6.0, 0.5, -1.0, 1.0, -1.0 ].

So the input is: `vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries`, and we have:
* `equations.size()` is always equals to `values.size()`
* the elements in the `values array` are all positive. This represents the equations that we already know
* we need to return `vector<double>`

According to the example above:
```
equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```
The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.

这题LeetCode标的是median难度，但如果要用union find 来做的话，我感觉已经是hard 难度。如果用DFS/BFS来做，难度是不高，但速度会慢不少

### Example
见上文

## Solution 1：用 Union Find 来做。难度高。速度快(前5%)。我死磕了一天一夜
这里用的 Union Find 是所有 UF 里比较高端第一种：用Tree的思想实现的，用group size做weight的，不断进行path compression的UF，而且还
带上了各个element之间的ratio值。我在这个repo里专门有一篇md记录 UF [在此](https://github.com/HungryOrc/hungryorc.github.io/edit/master/pages/algorithm/algorithm_union_find.md)，这个解法里用到的 helper class `PathCompressionWeightedTreeUnionFindWithRatiosBetweenElements` 就在上面这个文档里写了，是末尾的单独的一个小节。在这里就不重写这个 UF class 了

### Complexity
* Time: 得到每一个新的 `x/y` 的耗时：armortized O(1) <=== 对么 ？？？
* Space: O(n) <=== 对么 ？？？

### Java
```java
public class Solution {
    
    // 这个函数里没什么特别的东西，逻辑简单，精华都在 class UnionFind 里
    public double[] calcEquation(String[][] equations, double[] values, 
            String[][] queries) {
        
        // 真正不重复的元素一定没有 equations.length * 2 那么多个，因为equations里有重复元素，
        // 但直接这么写就不需要先找出一共有多少个不重复的元素，再设置UF，
        // 再从头捋一遍equations来实施各个unions
        PathCompressionWeightedTreeUnionFind uf = 
            new PathCompressionWeightedTreeUnionFind(equations.length * 2);
        
        int index = 0;
        // <string value, index for this string for union find>
        Map<String, Integer> map = new HashMap<>();
        
        // 把已知的所有关系搞进UF
        for (int i = 0; i < equations.length; i++) {
            String[] pair = equations[i];
            String s1 = pair[0];
            String s2 = pair[1];
            
            Integer index1 = map.get(s1);
            Integer index2 = map.get(s2);
            if (index1 == null) {
                index1 = index;
                map.put(s1, index1);
                index++;
            }
            if (index2 == null) {
                index2 = index;
                map.put(s2, index2);
                index++;
            }

            uf.union(index1, index2, values[i]);
        }
        
        // 考察queries里面的关系
        double[] result = new double[queries.length];
        for (int i = 0; i < queries.length; i++) {
            String[] pair = queries[i];
            String s1 = pair[0];
            String s2 = pair[1];
            
            Integer index1 = map.get(s1);
            Integer index2 = map.get(s2);
            if (index1 == null || index2 == null) {
                result[i] = -1.0;
                continue;
            }
            
            if (s1.equals(s2)) {
                result[i] = 1.0;
                continue;
            }
            
            result[i] = uf.divideAByB(index1, index2);            
        }
        return result;
    }
}
```

## Reference
* [Evaluate Division [LeetCode]](https://leetcode.com/problems/evaluate-division/description/)

{% include links.html %}
