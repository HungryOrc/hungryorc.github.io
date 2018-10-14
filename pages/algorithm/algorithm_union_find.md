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
Maitain an int array of size n, array[i] is the group number of the i-th object, 
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
    
    // "find" means to check if two objects belong to the same group
    private boolean find(int a, int b) {
        return groupIDs[a] == groupIDs[b];
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
Maitain an int array of size n, array[i] is the group number of the i-th object, 
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
    
    // "find" means to check if two objects belong to the same group
    private boolean find(int a, int b) {
        return groupIDs[a] == groupIDs[b];
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



{% include links.html %}
