---
title: "Longest Path in a Flights Graph which has only one Starting point, tree structure, and no cycle"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_longestPath_flightsGraph_oneStart.html
folder: algorithm
toc: false
---

## Description
全图有且只有一个初始点，除了他以外的点都有previous city。每个city只能有一个previous city。每个city可以有0到多个next cities。全图没有cycle。综上所述，这就是一个树。

求最长的path的具体沿线cities。

### Example
略

## Solution: DFS 我的实现

### Complexity
* Time: O(|E|) <==== ？？？？
* Space: O(|E|) <==== ？？？？

### Java
```java
class Solution {
    public List<String> getLongestPath(String[][] flights) {
        // ignore sanity checks
        
        // these 2 sets are used for finding the global starting city
        // end set 是永远只加不减
        // start set 是有加有减
        Set<String> starts = new HashSet<>();
        Set<String> ends = new HashSet<>();
        
        // <start city, next cities>
        Map<String, List<String>> nexts = new HashMap<>();
        
        for (String[] flight : flights) {
            String start = flight[0], end = flight[1];
            
            if (!ends.contains(start)) {
                starts.add(start);
            }
            
            if (starts.contains(end)) {
                starts.remove(end);
            }
            ends.add(end);
            
            List<String> nextCities = nexts.get(start);
            if (nextCities == null) {
                nextCities = new ArrayList<>();
                nexts.put(start, nextCities);
            }
            nextCities.add(end);
        }
        
        // get the global starting city,
        // for now the set for the starts will must have and only have one city
        String globalStart = "";
        for (String s : starts) {
            globalStart = s;
        }
        
        // begin to find the longest path
        List<String> curPath = new ArrayList<>();
        curPath.add(globalStart);
        List<String> longestPath = new ArrayList<>();
        dfs(nexts, globalStart, curPath, longestPath);
        return longestPath;        
    }
    
    private void dfs(Map<String, List<String>> nexts, String curCity, 
            List<String> curPath, List<String> longestPath) {
        if (curPath.size() > longestPath.size()) {
            longestPath.clear();
            for (String s : curPath) longestPath.add(s); // 整个copy一遍
            
            // 特别注意！这里不要用 longestPath = new ArrayList<>(curPath) ！！！
            // 我错在这里，debug了很久！
            // 如果这么new一个list来实现copy，那么其实每次都造了一个船新的longestPath！
            // 和之前的longestPath是两个不同的object！就是说每条DFS上的longestPath都
            // 互相之间完全没关系了！
        }
        
        List<String> nextCities = nexts.get(curCity);
        if (nextCities == null || nextCities.size() == 0) return;
        
        for (String next : nextCities) {
            curPath.add(next);
            
            dfs(nexts, next, curPath, longestPath);
            
            curPath.remove(curPath.size() - 1); // 复原
        }
    }
}
```

## Reference
Uber电面题，网上没看见

{% include links.html %}
