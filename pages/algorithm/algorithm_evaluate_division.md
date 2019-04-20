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
带上了各个element之间的ratio值。我在这个repo里专门有一篇md记录 UF [在此](https://github.com/HungryOrc/hungryorc.github.io/edit/master/pages/algorithm/algorithm_union_find.md)。

### Complexity
* Time: 得到每一个新的 `x/y` 的耗时：armortized O(1) <=== 对么 ？？？
* Space: O(n) <=== 对么 ？？？

### Java
```java
public class Solution {
    
    // 这个函数里没什么特别的东西，逻辑简单，精华都在 class UnionFind 里
    public double[] calcEquation(String[][] equations, double[] values, String[][] queries) {
        
        // 真正不重复的元素一定没有 equations.length * 2 那么多个，因为equations里有重复元素，但直接这么写简单，
        // 这么写就不需要先找出一共有多少个不重复的元素，再设置UF，再从头捋一遍equations来实施各个unions
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

// With Ratios between elements
class PathCompressionWeightedTreeUnionFind {
    public int[] parentIDs;
    private int[] groupSizes;
    public double[] ratios; // 当前元素 除以 group root 元素，等于多少，就是这个ratio值

    public PathCompressionWeightedTreeUnionFind(int n) {
        this.parentIDs = new int[n];
        this.groupSizes = new int[n];
        this.ratios = new double[n];
        
        for (int i = 0; i < n; i++) {
            parentIDs[i] = i;
            groupSizes[i] = 1;
            ratios[i] = 1.0; // 一开始对于每个元素来说，它的root就是它自己，所以ratio也都是 1.0（自己除以自己）
        }
    }
    
    public int getGroupID(int index) {
        int curIndex = index;
        List<Integer> list = new ArrayList<>();
        
        // step 1, get group ID, 顺便也把所有 直系父辈 的indexes都拿到，放到list里，
        // list从左到右依次是：自己，老豆，爷爷，太爷...
        while (curIndex != parentIDs[curIndex]) {
            list.add(curIndex);
            curIndex = parentIDs[curIndex];
        }
        int groupID = curIndex;
        
        // step 2, update the parent id of the object and all its direct ancestors to be the group id,
        // and also update the ratios of the object and all its direct ancestors
        int n = list.size();
        // 从倒数第二个元素开始，因为倒数第一个元素的parent就是root，所以倒数第一个元素的ratio是不需要更改的
        for (int i = n - 2; i >= 0; i--) {
            // 更新ratio值
            curIndex = list.get(i);
            int parentIndex = list.get(i + 1);
            ratios[curIndex] *= ratios[parentIndex];
            // 更新parent id 为 root id 即 group id
            parentIDs[curIndex] = groupID;
        }
        return groupID;
    }

    public boolean find(int a, int b) {
        int groupIDA = getGroupID(a);
        int groupIDB = getGroupID(b);
        
        return groupIDA == groupIDB;
    }

    public void union(int a, int b, double ratioADivideByB) {
        int groupIDA = getGroupID(a);
        int groupIDB = getGroupID(b);
        
        if (groupIDA == groupIDB) {
            return;
        }

        if (groupSizes[groupIDA] >= groupSizes[groupIDB]) {
            parentIDs[groupIDB] = groupIDA;
            groupSizes[groupIDA] += groupSizes[groupIDB];
            
            // 更新 b 和 b的所有直系父辈 的ratio，以及group id
            double ratioBDivideByA = 1.0 / ratioADivideByB;
            int curIndex = b;
            double ratioFactor = (ratioBDivideByA * ratios[a] / ratios[b]);
            while (parentIDs[curIndex] != curIndex) {
                int parentID = parentIDs[curIndex];
                ratios[curIndex] *= ratioFactor;
                parentIDs[curIndex] = groupIDA;
                curIndex = parentID;
            }
        } 
        else {
            parentIDs[groupIDA] = groupIDB;
            groupSizes[groupIDB] += groupSizes[groupIDA];
            
            // 更新 a 和 a的所有直系父辈 的ratio，以及group id
            int curIndex = a;
            double ratioFactor = ratioADivideByB * ratios[b] / ratios[a];
            while (parentIDs[curIndex] != curIndex) {
                int parentID = parentIDs[curIndex];
                ratios[curIndex] *= ratioFactor;
                parentIDs[curIndex] = groupIDB;
                curIndex = parentID;
            }
        }
    }
    
    public double divideAByB(int a, int b) {
        // 如果a和b不在一个group里，则它们之间不可相除，因为它们之间根本毫无关联
        if (!find(a, b)) {
            return -1.0;
        }
        
        // 到了这里的时候，a和b的ratio值必然都是它们相对于它们的(共同的)root element 的倍数值了
        return ratios[a] / ratios[b];
    }
}
```

## Reference
* [Evaluate Division [LeetCode]](https://leetcode.com/problems/evaluate-division/description/)

{% include links.html %}
