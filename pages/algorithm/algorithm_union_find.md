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

## Overview
A set of objects, represented by integers 0 to n-1, without loss of generality. You can:
* Connect any two of them to be in the same group, using `void union(int a, int b)`
* Query whether any two of them are in the same group, using `boolean find(int a, int b)`

## Implementation Stage 1: Union by an Int Array
Maintain an int array of size n, array[i] is the **Group Number (not Parent ID)** of the i-th object, 
so if object A and object B are in the same group, then array[A] == array[B], vise versa.

### Java
```java
class ArrayUnionFind {
    private int[] groupIDs;
    
    // n is the number of objects
    public ArrayUnionFind(int n) {
        this.groupIDs = new int[n];
        for (int i = 0; i < n; i++) {
            groupIDs[i] = i; // initialization
        }
    }
    
    public int getGroupId(int a) {
        return groupIDs(a);
    }
    
    // "find" means to check if two objects belong to the same group
    public boolean find(int a, int b) {
        return groupIDs[a] == groupIDs[b];
    }
    
    // merge a and b into the same group,
    // ALSO merging the objects that are already in the same group as a or b!
    // O(n) time
    public void union(int a, int b) {
        int groupIDA = groupIDs[a];
        int groupIDB = groupIDs[b];
        
        if (groupIDA == groupIDB) {
            return;
        }
        
        for (int i = 0; i < groupIDs.length; i++) { // 一个一个查，一个一个写
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
Maintain an int array of size n, array[i] is the **Parent ID** of the i-th object, **not the Group ID** (namely the index of the "**root**" object of this group) of the i-th object!
* 如果一个object的parent等于它自己，那么意味着这个object就是一个group的root

And when we meger two groups, we set either one group's root to be the **DIRECT** child of the other group's root

在这种实现方式里
* 每次做 `find(int a, int b)`，**都要先找 a 和 b 的 root**
* 每次做 `union(int a, int b)`，**也都要先找 a 和 b 的 root**

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
    public int getGroupID(int a) {
        int curIndex = a;
        while (curIndex != parentIDs[curIndex]) {
            curIndex = parentIDs[curIndex];
        }
        return curIndex;
    }
    
    // "find" means to check if two objects belong to the same group
    public boolean find(int a, int b) {
        int groupIDA = getGroupID(a); // time: O(n)
        int groupIDB = getGroupID(b); // time: O(n)
        
        return groupIDA == groupIDB;
    }
    
    // merge a and b into the same group
    public void union(int a, int b) {
        int groupIDA = getGroupID(a); // time: O(n)
        int groupIDB = getGroupID(b); // time: O(n)
        
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
基于前述的 Tree Union Find 的改进。这里的weighted是指一个tree里面的元素的个数，即tree的size，size越大则这个所谓的weight越大。
我们要union两个group即两个tree的时候，谁的size小，就把它的root设为另一个size更大的tree的root的direct child。这样做最终能减少union后所得到的tree的高度。

### Java
```java
class WeightedTreeUnionFind {
    // 这个数组表示往上一层的object，不表示本group的最高层object！
    private int[] parentIDs;
    
    private int[] groupSizes; // <=== this is the new stuff!
    
    // n is number of objects
    public WeightedTreeUnionFind(int n) {
        this.parentIDs = new int[n];
        this.groupSizes = new int[n];
        
        for (int i = 0; i < n; i++) {
            parentIDs[i] = i;
            groupSizes[i] = 1; // <=== new stuff
        }
    }
    
    public int getGroupID(int a) {
        int curIndex = a;
        while (curIndex != parentIDs[curIndex]) {
            curIndex = parentIDs[curIndex];
        }
        return curIndex;
    }

    public boolean find(int a, int b) {
        int groupIDA = getGroupID(a); // time: O(n)
        int groupIDB = getGroupID(b); // time: O(n)
        
        return groupIDA == groupIDB;
    }

    public void union(int a, int b) { // <=== this function changed!
        int groupIDA = getGroupID(a); // time: O(n)
        int groupIDB = getGroupID(b); // time: O(n)
        
        if (groupIDA == groupIDB) {
            return;
        }
        
        if (groupSizes[groupIDA] >= groupSizes[groupIDB]) {
            parentIDs[groupIDB] = groupIDA;
            groupSizes[groupIDA] += groupSizes[groupIDB];
            // since groupIDB will no longer be a root for any group,
            // so no body will care about its size, so we don't need to update it
        } else {
            parentIDs[groupIDA] = groupIDB;
            groupSizes[groupIDB] += groupSizes[groupIDA];
        }
    }
}
```

### Complexity
* Time: 
  * Union: O(tree height) = O(logn)
  * Find: O(tree height) = O(logn)
  * 证明 tree height == logn:
    * 首先，显然 h(1) = 1 <= log_2(1) + 1
    * 基于以上，我们要证：h(n) <= log_2(n) + 1
    * 可以这么做：假设 h(p) <= log_2(p) + 1 而且 h(q) <= log_2(q) + 1，要证明 h(p+q) <= log_2(p+q) + 1
* Space: O(n), the size of the int array


## Implementation Stage 4: Path Compression Weighted Tree Union Find
基于前述的 Tree Union Find 的改进。也是要降低树的高度，但是方式更猛：不断地把各个object直接接在当前group的root object的下面。

但要特别注意：这种给root做儿子的操作不是无时无刻在进行的！只是在每次对一个object A 做getRoot即getGroupID操作的时候，才把A和A的各级 **直系 parent** 都设为这个root。
* 这个压缩高度的操作**并不惠及兄弟和叔伯！也不惠及所有直系和旁系的子孙！只惠及它自己本身以及它的各级直系父亲！**
* 所以比如在做union(a, b)的时候，
  * 先分别对a和b做这种认root做父的操作
  * 然后把这两个tree连在一起，即把size较小的tree的root变成较大的tree的root的 direct child

### Java
```java
class PathCompressionWeightedTreeUnionFind {
    private int[] parentIDs;
    private int[] groupSizes;

    public PathCompressionWeightedTreeUnionFind(int n) { // <=== this function unchanged
        this.parentIDs = new int[n];
        this.groupSizes = new int[n];
        
        for (int i = 0; i < n; i++) {
            parentIDs[i] = i;
            groupSizes[i] = 1;
        }
    }
    
    public int getGroupID(int index) {
        int curIndex = index;
        
        // step 1, get group ID
        while (curIndex != parentIDs[curIndex]) {
            curIndex = parentIDs[curIndex];
        }
        int groupID = curIndex; // namely the index of root object
        
        // <=== this is the new stuff!!
        // step 2, update the parent id of the object and all its direct ancestors to be
        // the group id, namely the id of the root object
        curIndex = index;
        while (curIndex != groupID) {
            int parentID = parentIDs[curIndex];
            parentIDs[curIndex] = groupID;
            curIndex = parentID;
        }
        // 特别注意！上面这个压缩高度的操作并不惠及兄弟和叔伯！所以他们在此时并不能沾光变矮！
        
        return groupID;
    }

    public boolean find(int a, int b) { // <=== this function unchanged
        int groupIDA = getGroupID(a);
        int groupIDB = getGroupID(b);
        
        return groupIDA == groupIDB;
    }

    public void union(int a, int b) { // <=== this function unchanged
        int groupIDA = getGroupID(a);
        int groupIDB = getGroupID(b);
        
        if (groupIDA == groupIDB) {
            return;
        }
        
        if (groupSizes[groupIDA] >= groupSizes[groupIDB]) {
            parentIDs[groupIDB] = groupIDA;
            groupSizes[groupIDA] += groupSizes[groupIDB];
        } else {
            parentIDs[groupIDA] = groupIDB;
            groupSizes[groupIDB] += groupSizes[groupIDA];
        }
    }
}
```

### Complexity
* Time:
  * Union: O(1), armortized! Not every time everywhere O(1)!
  * Find: O(1), armortized! Not every time everywhere O(1)!
  * 证明非常困难，记住结论即可
* Space: O(n), the size of the int array

## Implementation Stage 5: 包含了元素之间的ratios 的 Path Compression Weighted Tree Union Find
这个implementation是基于上一个implementation来进行的。它是为了满足一种特殊的要求：即
* 元素之间有ratios，我们已知一些元素之间的ratios
* 要求其他的元素之间的ratios（如果这个ratio不存在即这两个元素不属于同一个group，则返回 -1.0）
* LeetCode上有一道题就是这种类型，叫[Eveluate Division](https://leetcode.com/problems/evaluate-division/description/)

### 实施细节
每个元素要带上一个double值，表达它的ratio。具体来说，这个ratio是这个元素所在的group的root元素的值 除以 这个元素的值 得到的商

#### Union
* 每次我们拿到一个新的比例关系的时候，比如 `a/b = 2.5`，我们要把它表达到UF里面去。方式就是用 `union` 来做。因为此时a和b算是在同一个group里了
* 同一个group里的元素之间都有ratio关系。不过如上一条所说，它们具体记录下来的ratio值规定为 它们与root元素的比值
* 在一个group里，任何新加入的元素比例关系不能破坏原有的关系，否则无效。比如我们已知 a/b=2.0，b/c=3.0，但如果再来一个 a/c=5.5，那么新来的这个是不可接受的。在这里我们假定所有新加入的比例关系都不破坏原有的所有关系
* 一个新来的比例关系可能会往现有的group里增加一个元素。还有可能把2个现有的group连接在一起。比如现有a,b,c 和d,e,f,g,h 这两个groups。如果来了一个比例关系是 e/b = 5.0，则a,b,c,d,e,f,g,h 就全连成一个group了。那么部分元素的ratio值就要调整。见下面的例子：
* 已有的关系（数字表示本元素的值是本group的root元素的值的 多少倍）：
  ```
  a(1.0, root of group a)         d(1.0, root of group d)
     |                                /       \
  b(0.5)                          e(0.33)     f(0.25)
     |                                        /    \
  c(0.25)                                g(0.5)     h(2.0)
  ```
* 新加的关系：e 是 b 的5倍：
  ```
  e(1.0)
    |
  b(0.2)
  ```
* 因为b所在的group的size是3，小于e所在的group的size(5)，所以要把b的root即a并到e的root即d的下面。a将成为d的一个新的直接儿子
* 难点在于，**b的ratio在新的以d为root的tree里要改变为 0.33(e在d树里的ratio) * 0.2(b/e的值) = 0.066**。还有哪些元素的ratio要改？怎么改？
  * **b的ratio要从0.5 变成 0.066，那么 b的所有直系父辈的ratio 都要按这个幅度变，即它们的ratio都要 除以0.5，再乘以0.066**
  * **同时，b和b的所有直系父辈的 parent id 都变为 d**
  * **其他的所有元素的ratio都不变，parent id 也不变**
* 合并以后的group：
  ```
          d(1.0, root of group d)
        /       |         |       \
  b(0.066)   a(0.132)   e(0.33)   f(0.25)
     |                             /    \
  c(0.25)                       g(0.5)   h(2.0)
  ```  

#### Get Root for an Element
* 对于一个元素a，getRoot(a) 的过程里，按照path compression的惯例，是要把a和a的所有直系父辈的parent id 都设为本group的group id。而现在我们对于每个元素都有ratio值，所以这个过程里也要把这些元素的ratio值都更新一下。举例如下图，右边是做了 get root for a 之后的树：
  ```
         f(1.0)                                     f(1.0)
      /    |    \                       /    |      |          |        \
  b(2.5)  c(2)   g(4.1)             b(2.5)  c(2)  h(2*0.3)  a(2*0.3*7)  g(4.1)
         /   \      |                        |      |          |          |
     h(0.3)  i(6)  k(0.66)   ===>           i(6)  j(0.1)     t(9.2)     k(0.66)
      /  \          |                               |                     |
   a(7)  j(0.1)    m(5)                           p(3.4)                m(5)
    |      |                                                        
  t(9.2)  p(3.4)                                             
  ```

#### Find
* 这种UF的`find(a, b)` 和一般的 Path Compression UF 没有区别。一样是先get root for a and b respectively（在这个过程里已经偷偷地做了上文提及的那些事），然后compare这两个root是否相等

#### Get an Unknown Ratio of x/y
* 先做 `find(x, y)`。如果x和y不在一个group里，则返回 -1.0
* 再看a和b的ratio分别是多少，把它们相除就行了。因为刚才在find函数运行的时候，已经偷偷把它们两的ratio值都更新了，都是相对于同一个root的ratio

### Java
```java
class PathCompressionWeightedTreeUnionFindWithRatiosBetweenElements {
    public int[] parentIDs;
    private int[] groupSizes;
    public double[] ratios; // 当前元素 除以 group root 元素，等于多少，就是这个ratio值

    public PathCompressionWeightedTreeUnionFindWithRatiosBetweenElements(int n) {
        this.parentIDs = new int[n];
        this.groupSizes = new int[n];
        this.ratios = new double[n];
        
        for (int i = 0; i < n; i++) {
            parentIDs[i] = i;
            groupSizes[i] = 1;
            // 一开始对于每个元素来说，它的root就是它自己，所以ratio也都是 1.0（自己除以自己）
            ratios[i] = 1.0;
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
        
        // step 2, update the parent id of the object and all its direct ancestors 
        // to be the group id, and also update the ratios of them
        int n = list.size();
        // 从倒数第二个元素开始，因为倒数第一个元素的parent就是root，
        // 所以倒数第一个元素的ratio是不需要更改的
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
        
        // 到了这里的时候，a和b的ratio值必然都是它们相对于它们的(共同的)root element的倍数值了
        return ratios[a] / ratios[b];
    }
}
```

### Complexity
* Time:
  * Union: O(1), armortized <=== 对么 ？？？
  * Find: O(1), armortized <=== 对么 ？？？
  * 证明非常困难，记住结论即可
* Space: O(n), the size of the int array


{% include links.html %}
