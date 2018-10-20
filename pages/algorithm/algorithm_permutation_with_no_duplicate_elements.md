---
title: "Permutation with no Duplicate Elements"
tags: [algorithm, recursion]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_permutation_with_no_duplicate_elements.html
folder: algorithm
toc: false
---

## Description
Given a collection of **distinct** integers, return all possible permutations.
* 数组里的元素不重复
* 每个元素必须出现且只出现一次
* 元素之间的相对顺序 matters

### Example
* Input: [1,2,3]
  * Output:
  ```
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
  ```

## Solution 1: DFS + Swap，速度前 1% ！
从左到右逐个处理数组里的每一个位置
* 最左边的一个位置有n种可能，用for loop 和 swap来实现
* 左边第二个位置就只有n-1种可能了，当前处于最左边的那一位就不能参与选妃了。也用for loop和swap来实现
* 继续往右搞......

### Complexity
* Time: O(n * n!)
  * 每个答案所需的 n 的时间是消耗在：每个答案得到以后，要用n的时间来造new List来固化这个答案
  * 得到每个答案的时间，其实只需要O(1)，因为通过 swap 方法，具体解释如下：
    * 比如说要找 1, 2, 3 这三个数一共能组成多少个排列（答案是6种），那么swap方法的解题过程其实是：
      ```
         1         2         3
        / \       / \       / \
       2   3     1   3     1   2
      ```
    * 再往下还有一层，但前两层定了以后，第三层就已经定了
    * 定前两层的时间消耗，可以看出，是 O(6)，与答案的个数是一个量级的！
  * 所以一共有n!种答案，然后n!种答案一共也只需要n!的时间来得到。这也就是 **swap 方法 炒饭脱俗地迅捷 的原因 yeah**
  * 不过在数学上说，n*n! 和 n! 其实是一个量级的，所以这一题的答案也可写为 O(n!)
* Space: O(n)
  * Since we need n call stacks, and each call stack requires constant space

### Java
```java

```

## Reference
* [文章标题 [LeetCode]](网址放在这里)

{% include links.html %}
