---
title: "Assign Cookies"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_assign_cookies.html
folder: algorithm
toc: false
---

## Description
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. 
Your goal is to maximize the number of your content children and output the maximum number.

Note
* You may assume the greed factor is always positive. 
* You cannot assign more than one cookie to one child.

### Example
* Input: [1,2,3], [1,1]
  * Output: 1. You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
* Input: [1,2], [1,2,3]
  * Output: 2. You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.

## Solution 1: Greedy Algorithm
从最小胃的孩子开始，尝试用最小的cookie去满足最小胃的孩子。所以这是一个贪心算法，其局部最优解我们“认为”等于全局最优解……

无法满足的崽子，就不用给任何cookie了，因为最后只要求满足的个数

另外这一题还有个特点，就是满足一个大胃的小孩，和满足一个小胃的小孩，回报都是一样的，都是1。所以用贪心算法ok。如果满足大胃小孩的回报高于满足小胃小孩，
那就应该用下面的第二种解法：从最大胃的孩子开始，尝试用最小的cookie去满足最大胃的孩子

### Complexity
* Time: O(nlogn)，这是排序的时间。处理的时间只有 O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int findContentChildren(int[] children, int[] cookies) {
        // ignore validations
        
        Arrays.sort(children);
        Arrays.sort(cookies);
        
        int n = children.length;
        int m = cookies.length;
        int i = 0, j = 0;
        
        int count = 0;
        while (i < n && j < m) {
            if (children[i] <= cookies[j]) {
                count++;
                i++;
                j++;
            } else {
                j++;
            }
        }
        return count;
    }
}
```

## Solution 2: 更伟光正的方法，能确保全局最优解
从最大胃的孩子开始，尝试用最小的cookie去满足最大胃的孩子

### Complexity
* Time: O(n^2)
* Space: O(1)

### Java
```java
public class Solution {
    public int findContentChildren(int[] children, int[] cookies) {
        // ignore validations
        
        Arrays.sort(children);
        Arrays.sort(cookies);
        int n = children.length;
        int m = cookies.length;
        
        int count = 0;
        for (int i = n - 1; i >= 0; i--) { // 从大胃小孩开始
            for (int j = 0; j < m; j++) { // 从小曲奇开始
                if (cookies[j] == -1) { // -1 意味着这个cookie被吃掉了
                    continue;
                }
                
                if (children[i] <= cookies[j]) {
                    cookies[j] = -1;
                    count++;
                    break;
                }
            }
        }
        return count;
    }
}
```

## Reference
* [Assign Cookies [LeetCode]](https://leetcode.com/problems/assign-cookies/description/)

{% include links.html %}
