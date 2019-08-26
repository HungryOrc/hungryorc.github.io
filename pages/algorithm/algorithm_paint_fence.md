---
title: "Paint Fence"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_paint_fence.html
folder: algorithm
toc: false
---

## Description
There is a fence with n posts, each post can be painted with one of the k colors.
You have to paint all the posts such that no more than two adjacent fence posts have the same color.

Return the total number of ways you can paint the fence.

Note:
* n and k are non-negative integers.

### Example
* Input: n = 3, k = 2
  * Output: 6
  ```
           post1  post2  post3           
   1         c1     c1     c2 
   2         c1     c2     c1 
   3         c1     c2     c2 
   4         c2     c1     c1  
   5         c2     c1     c2
   6         c2     c2     c1
  ```

## Solution: DP
不过似乎没有考虑如果fence是围成一圈，最后的posts和最开头的posts之间的重色问题

We divided it into two cases:
1. The latest 2 posts have the same color, the number of ways to paint in this case is `sameColorCounts`.
2. The latest 2 posts) have different colors, the number of ways in this case is `diffColorCounts`.

When adding a new post, we can use the same color as the last one (if allowed) or a different color. 
* If we use a different color, there're `k-1` options, and the outcomes shoule belong to the `diffColorCounts` category. 
* If we use the same color, there's only 1 option, and we can only do this when the last 2 have different colors (which is the `diffColorCounts`). 
So we can have our induction step.

Here is an example, let's say we have 3 posts and 3 colors. 
* The first 2 posts we have 9 ways to do them, (1,1), (1,2), (1,3), (2,1), (2,2), (2,3), (3,1), (3,2), (3,3)
* Now we have: diffColorCounts = 6, and sameColorCounts = 3
* Now for the 3rd post, we can compute these 2 variables like this:
  * If we use different colors than the last one (namely the second one here), this shall be added into `diffColorCounts`. Apparently there are `(diffColorCounts + sameColorCounts) * (k - 1)` possible ways.
  * If we use the same color as the last one, we would trigger a violation in the 3 cases (1,1,1), (2,2,2) and (3,3,3). The other 6 are ok. And now as we append a same-color post to each combinatoin, **the former diffColorCounts becomes the current sameColorCounts**.
* We can keep going until we reach the n. And finally just sum up these 2 variables

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int numWays(int n, int k) {
        if (n <= 0) {
            return 0;
        }
        if (n == 1) {
            return k;
        }
        if (n == 2) {
            return k * k;
        }
        
        int preTwoAreSame = k;
        int preTwoAreDiff = k * (k - 1);
        
        for (int i = 3; i <= n; i++) {
            int tmp = preTwoAreDiff;
            preTwoAreDiff = (preTwoAreDiff + preTwoAreSame) * (k - 1);
            preTwoAreSame = tmp;
        }
        return preTwoAreSame + preTwoAreDiff;
    }
}
```

## Reference
* [Paint Fence [LeetCode]](https://leetcode.com/problems/paint-fence/description/)

{% include links.html %}
