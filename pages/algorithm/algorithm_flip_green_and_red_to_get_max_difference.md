---
title: "Flip Green and Red to Get Maximimum |G| - |R|"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_flip_green_and_red_to_get_max_difference.html
folder: algorithm
toc: false
---

## Description
一个array，里面只有2种char：`G`和`R`。我们可以对任意一段sub array 做一次flip操作，把它里面的G全部变成R，R全部变成G。
目的是得到最大的 `G个数 - R个数`，这个是就整个array来说的。

### Example
* Input: "GRGGRRGRRRG"
  * Output: 7, flip 中间的 "RRGRRR" 这一段
* Input: "GGRGGRGRRR"
  * Output: 6, flip the last 3 R

## Solution：prefix sum
这个数组本来就是由纯粹的G和R组成的。那么它本来就会有一个 G - R 的值，设为 x。
如果我们把某个sub array 上的G和R都flip了，假设这个sub array一开始有m个G，n个R，那么flip以后，G的个数要 - m 然后 + n，R的个数正好反过来，
要 - n 然后 + m。所以flip之后 G - R = x + 2n - 2m。所以变量就是 2n - 2m，之前的x是一开始就固定了的无法改变的。所以我们就是要最大化 n - m。

就是说我们要找一个 sub array，里面 R的个数 减去 G的个数 的值越大越好。就是R要相对越多越好，G要相对越少越好。

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
public class Solution {
    public int maxDiffAfterFlip(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int n = s.length();
        
        // curDiff 表示 从index 0 开始到 cur index（含），|R| - |G| 这个值是多少
        // 我们要求的sub array的|R| - |G| 要越大越好，
        // 因为我们之后要反转这个sub array，以得到最大的 |G| - |R|
        //
        // 先把第一个char处理了
        int curDiff = (s.charAt(0) == 'R') ? 1 : -1;
        int minPrevDiff = 0;
        int maxDiff = curDiff;
        
        int countR = (s.charAt(0) == 'R') ? 1 : 0; 
        int countG = 1 - countR;
        
        // 从第二个char开始
        for (int i = 1; i < n; i++) {
            char c = s.charAt(i);
            
            if (c == 'R') {
                countR++;
                curDiff++;
            } else {
                countG++;
                curDiff--;
            }

            maxDiff = Math.max(maxDiff, curDiff - minPrevDiff);
            minPrevDiff = Math.min(minPrevDiff, curDiff);
        }
        return maxDiff * 2 + countG - countR; // 注意这里别忘了乘以2，见文本里的解释
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
