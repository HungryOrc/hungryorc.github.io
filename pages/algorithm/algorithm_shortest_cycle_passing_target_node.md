---
title: "Find the Shortest Cycle that Passes a Specific Node in the Graph"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_shortest_cycle_passing_target_node.html
folder: algorithm
toc: false
---

## Description
给一个directed graph，再给定图里的某一个node作为target，find the shortest cycle that passes this target node，
return the nodes in this cycle as a list。
如果没有任何cycle通过这个node，则返回空的list。

### Example
略

## Solution: BFS <---- 我的方法对么？？？？
可以用一个 `Map<Integer, Set<Integer>>` 来表示graph，其中 key Integer 表示一个node的value，Set<Integer> 表示这个node所指向的那些nodes（的values）。所以这种做法需要隐含条件是所有nodes的value不相等。不过也不要紧，我们可以把node的value改为node的id，
那么就一定可以做到不重复了

如果要用一个class Node 来表示nodes的话，也可以，但是如果要把这个custom class 放到 map 或者 set 里去，就需要给这个class写 equals 和 hashcode 这两个函数

### Complexity
* Time: O(|V| + |E|) <--- 对么 ？？？？
* Space: O(|V|) <--- 对么 ？？？？

### Java
```java
public class Solution {
    public List<Node> shortestCycle(Map<Integer, Set<Integer>> graph, int target) {
        if (graph == null || graph.size() == 0 || !graph.containsKey(target)) {
            return new ArrayList<Integer>();
        }
        
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> nodes = new LinkedList<>();
        Queue<List<Integer>> paths = new LinkedList<>();
        
        nodes.offer(target);
        
        List<Integer> list = new ArrayList<>();
        list.add(target);
        paths.offer(list);
        
        while (!nodes.isEmpty()) {
            int curSize = nodes.size();
            
            for (int i = 0; i < curSize; i++) {
                int curNode = nodes.poll();
                int curPath = paths.poll();
                
                if (curNode == target) {
                    return curPath;
                }
                
                visited.add(curNode);
                
                for (int neighbor : nodes.get(curNode)) {
                    if (visited(neighbor)) {
                        continue;
                    }
                    
                    nodes.offer(neighbor);
                    curPath.add(neighbor);
                    paths.offer(curPath);
                }
            }
        }
        return new ArrayList<Integer>(); // 没有cycle通过target
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
