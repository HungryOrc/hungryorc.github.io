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
 -----      -----  -----  -----       
   1         c1     c1     c2 
   2         c1     c2     c1 
   3         c1     c2     c2 
   4         c2     c1     c1  
   5         c2     c1     c2
   6         c2     c2     c1
  ```

## Solution: DP
    // 不过似乎没有考虑如果fence是围成一圈，最后的posts和最开头的posts之间的重色问题
    
    // 以下是上述算法的作者的解释：
    /* We divided it into two cases:
       1. 最近的两个柱子(the last two posts) have the same color, 
          the number of ways to paint in this case is sameColorCounts.
       2. 最近的两个柱子(the last two posts) have different colors, 
          the number of ways in this case is diffColorCounts.

       The reason why we have these two cases is that we can easily compute both of them, 
       and that is all I do. When adding a new post, we can use the same color as the last one (if allowed) or different color. 
       If we use different color, there're k-1 options, and the outcomes shoule belong to the diffColorCounts category. 
       If we use same color, there's only one option, 
       and we can only do this when the last two have different colors (which is the diffColorCounts). 
       There we have our induction step.

       Here is an example, let's say we have 3 posts and 3 colors. 
       The first two posts we have 9 ways to do them, (1,1), (1,2), (1,3), (2,1), (2,2), (2,3), (3,1), (3,2), (3,3). 
       Now we know that:
           diffColorCounts = 6;
       And
           sameColorCounts = 3;
       Now for the third post, we can compute these two variables like this:

       If we use different colors than the last one (the second one), 
       these ways can be added into diffColorCounts, so if the last one is 3, we can use 1 or 2, if it's 1, we can use 2 or 3, etc. 
       Apparently there are (diffColorCounts + sameColorCounts) * (k-1) possible ways.

       If we use the same color as the last one, we would trigger a violation in these three cases (1,1,1), (2,2,2) and (3,3,3). 
       This is because they already used the same color for the last two posts. 
       So is there a count that rules out these kind of cases? YES, the diffColorCounts. 
       So in cases within diffColorCounts, we can use the same color as the last one without worrying about triggering the violation. 
       And now as we append a same-color post to them, the former diffColorCounts becomes the current sameColorCounts.

       Then we can keep going until we reach the n. And finally just sum up these two variables as result.*/

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
public class Solution {
    public int numWays(int n, int k) {
        if(n == 0) return 0;
        else if(n == 1) return k;
        
        // When the number of posts is less than 2, there's no such concept as diffColorCount or sameColorCount 
        // because there's not enough to establish the rules.
        int diffColorCounts = k*(k-1);
        int sameColorCounts = k;
        
        for(int curPost=3; curPost<=n; curPost++) {
            int temp = diffColorCounts;
            diffColorCounts = (diffColorCounts + sameColorCounts) * (k-1);
            sameColorCounts = temp;
        }
        return diffColorCounts + sameColorCounts;
    }
}
```

## Reference
* [Paint Fence [LeetCode]](https://leetcode.com/problems/paint-fence/description/)

{% include links.html %}
