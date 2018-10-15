---
title: "Union Find"
tags: [algorithm, algorithm_overview, union_find]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_union_find.html
folder: algorithm
toc: false
---


path compression的做法，能做到合并两个group都能find const time，union const time 么？？？？！！！


## Overview
A set of objects, represented by integers 0 to n-1, without loss of generality. You can:
* Connect any two of them to be in the same group, using `void union(int a, int b)`
* Query whether any two of them are in the same group, using `boolean find(int a, int b)`

## Implementation Stage 1: Union by an Int Array
Maintain an int array of size n, array[i] is the group number of the i-th object, 
so if object a and object b are in the same group, then array[a] == array[b], vise versa.

### Java
```java
class ArrayUnionFind {
    private int[] groupIDs;
    
    // n is number of objects
    public ArrayUnionFind(int n) {
        this.groupIDs = new int[n];
        for (int i = 0; i < n; i++) {
            groupIDs[i] = i; // initialization
        }
    }
    
    private int getGroupId(int a) {
        return groupIDs(a);
    }
    
    // "find" means to check if two objects belong to the same group
    private boolean find(int a, int b) {
        return groupIDs[a] == groupIDs[b];
    }
    
    // merge a and b into the same group,
    // ALSO merging the objects that are already in the same group as a or b!!
    // this function costs O(n) time
    private void union(int a, int b) {
        int groupIDA = groupIDs[a];
        int groupIDB = groupIDs[b];
        for (int i = 0; i < groupIDs.length; i++) {
            if (groupIDs[i] == groupIDB) {
                groupIDs[i] = groupIDA;
            }
        }
    }
}
```

### Complexity
* Time: 
  * Union: O(n)
    * 原因是，比如a和b苟合，但是a和b可能都已经各自有了一群炮友，所以这可能会是两群炮友的苟合，更新任何一群都可能是O(n)，就算算法试图更新更小的那一群
    * 更有甚者，按照这种做法，我们根本不知道a和b的炮友们分别是哪些人，所以只能把整个数组遍历一遍
  * Find: O(1)
* Space: O(n), the size of the int array


## Implementation Stage 2: Tree Union Find
Maintain an int array of size n, array[i] is the **PARENT** of the i-th object, **not the group-number** (namely the index of the "**root**" object of this group) of the i-th object!
* 如果一个object的parent等于它自己，那么意味着这个object就是一个group的root

And when we meger two groups, we set either one group's root object to be the direct child of the other group's root object. We have an example below:

### Example
```
0 1 2 3 4 5 (indexes of the objects)

union(0, 3)
1 2 3 4 5
    | 
    0

union(2, 3)
1 3 4 5
 / \
0   2

union(4, 0)
1 3 5
 /|\
0 2 4

union(1, 5)
  3    5
 /|\   |
0 2 4  1

union(2, 1)
   5
 /   \
1     3
     /|\ 
    0 2 4
```

### Java
```java
class TreeUnionFind {
    // 这个数组表示往上一层的object，不表示本group的最高层object！
    private int[] parentIDs;
    
    // n is number of objects
    public TreeUnionFind(int n) {
        this.parentIDs = new int[n];
        for (int i = 0; i < n; i++) {
            parentIDs[i] = i; // initialization
        }
    }
    
    // namely get the index of the root object
    private int getGroupID(int a) {
        int curIndex = a;
        while (curIndex != parentIDs[curIndex]) {
            curIndex = parentIDs[curIndex];
        }
        return curIndex;
    }
    
    // "find" means to check if two objects belong to the same group
    private boolean find(int a, int b) {
        int groupIDA = getGroupID[a]; // time: O(n)
        int groupIDB = getGroupID[b]; // time: O(n)
        
        return groupIDA == groupIDB;
    }
    
    // merge a and b into the same group
    private void union(int a, int b) {
        int groupIDA = getGroupID[a]; // time: O(n)
        int groupIDB = getGroupID[b]; // time: O(n)
        
        groupIDs[groupIDA] = groupIDB; // time: O(1)
    }
}
```

### Complexity
* Time: 
  * Union: O(n)
  * Find: O(n)
  * 可见，在时间效率方面，看起来讨巧的tree方式，反而全面不如 前面第一种看起来最笨的group ID array方式。
    * 其中的原因是：tree方式的union那一下虽然很快，但是union之前（同理find之前）找root的那一步特别慢，是O(n)。所以优化的关键点在于加快找root的速度
    * 所以一种自然的优化思路就是降低tree的高度
* Space: O(n), the size of the int array


## Implementation Stage 3: Weighted Tree Union Find
基于前述的 Tree 


### Java
```java
class TreeUnionFind {
    // 这个数组表示往上一层的object，不表示本group的最高层object！
    private int[] parentIDs;
    
    // n is number of objects
    public TreeUnionFind(int n) {
        this.parentIDs = new int[n];
        for (int i = 0; i < n; i++) {
            parentIDs[i] = i; // initialization
        }
    }
    
    // namely get the index of the root object
    private int getGroupID(int a) {
        int curIndex = a;
        while (curIndex != parentIDs[curIndex]) {
            curIndex = parentIDs[curIndex];
        }
        return curIndex;
    }
    
    // "find" means to check if two objects belong to the same group
    private boolean find(int a, int b) {
        int groupIDA = getGroupID[a]; // time: O(n)
        int groupIDB = getGroupID[b]; // time: O(n)
        
        return groupIDA == groupIDB;
    }
    
    // merge a and b into the same group
    private void union(int a, int b) {
        int groupIDA = getGroupID[a]; // time: O(n)
        int groupIDB = getGroupID[b]; // time: O(n)
        
        groupIDs[groupIDA] = groupIDB; // time: O(1)
    }
}
```

### Complexity
* Time: 
  * Union: O(n)
  * Find: O(n)
  * 可见，在时间效率方面，看起来讨巧的tree方式，反而全面不如 前面第一种看起来最笨的group ID array方式。
    * 其中的原因是：tree方式的union那一下虽然很快，但是union之前（同理find之前）找root的那一步特别慢，是O(n)。所以优化的关键点在于加快找root的速度
    * 所以一种自然的优化思路就是降低tree的高度
* Space: O(n), the size of the int array




{% include links.html %}
