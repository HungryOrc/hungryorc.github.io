---
title: "Find 'Good' Internal Nodes"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_good_internal_nodes.html
folder: algorithm
toc: false
---

## Description：这题是(1/5/2019)周六mock题目，最后存档也没给答案，给Jennifer或Yan老师看下这么写是否ok ？？？
有一种二叉树。它的leaf nodes 都有label，要么"good"，要么"bad"；它的internal nodes 的label 都是""。
对于所有的internal nodes，我们做如下的判定（以下都不适用于leaf nodes）：
* 如果left或right child为null，则cur node 为bad
* 如果left或right child都是leaf
  * 如果它们都是good，则cur node 为good
  * 如果它们两个任一个是bad，则cur node 为bad
* 如果left货right child中的任一个是 internal node
  * 如果任一个 internal child node 为bad，则cur node 为bad
  * 必须所有的internal child node以及leaf child node都是good，那么 cur node 才能是good

求：所有的满足以下条件的internal nodes：
* 它自己是 good
* 它的parent 是 bad

### Example
a(bad) means a is internal
 f:good means f is leaf
``` 
                   a(bad)
                 /        \
           b(good)         c(bad)
          /      \        /      \
      e(good)  f:good   d:bad  g(bad)
     /     \                        \
 h:good  i:good                    j:good
```

## Solution
哦也

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java

```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
