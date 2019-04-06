---
title: "Rotate Digits to Count Good Numbers"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_rotate_digits_to_count_good_numbers.html
folder: algorithm
toc: false
---

## Description
X is a good number if after rotating each digit individually by 180 degrees, we get a valid number that is different from X.  Each digit must be rotated - we cannot choose to leave it alone.

A number is valid if each digit remains a digit after rotation. 0, 1, and 8 rotate to themselves; 2 and 5 rotate to each other; 6 and 9 rotate to each other, and the rest of the numbers do not rotate to any other number and become invalid.

Now given a positive number N, how many numbers X from 1 to N are good?

### Example
* Input: 10
  * Output: 4. There are four good numbers in the range [1, 10] : 2, 5, 6, 9. 
    * Note that 1 and 10 are NOT good numbers, since they remain unchanged after rotating.

## Solution：DP，速度 前10%
这题应该不难想到应该用 DP 来做，因为这题有很多sub question，它们的答案是可以记忆化并重复利用的。比如要查 12 是不是good，就要看1和2是不是good。
要查12345 是不是good number，就要看 1234 和 5 是不是good。

不过这题的具体DP处理方式是比较巧妙的，值得反复把玩。这是一种标注体系，标注 1，2，0 分别有不同的含义，具体的思路和流程 看下面代码里的注释

### Complexity
* Time: O(n)
* Space: O(n)，dp数组

### Java
```java
class Solution {
    public int rotatedDigits(int n) { // n is guaranteed to > 0
        int count = 0;
        int[] dp = new int[n + 1]; // 表示从0到n 这n+1个数，它们中的每一个 是不是 good number
        
        // 虽然n一定>0，但0对于后面的数是有用的！所以必须也考察0
        for (int i = 0; i <= n; i++) {

            // Step 1, 先写好0到9
            if (i < 10){
                
                // Type 1 digit
                // 用1表示：这一位本身是good number，但rotate后还是自己，所以一个数如果只有这种位的话，
                // 这个数还是不算是 good number！因为good number必须是rotate以后还是一个合法的数且不等于自己的原值
                if (i == 0 || i == 1 || i == 8) {
                    dp[i] = 1;
                }
                
                // Type 2 digit
                // 用2表示：这一位能确保本身是good number，而且rotate后和原值不同
                // 那么一个数里如果有至少一位这样的数字，其他位都是type 1 digit（或者type 2 digit），则这整个数就是good number了
                else if (i == 2 || i == 5 || i == 6 || i == 9) {
                    dp[i] = 2;
                    count++;
                }
                
                // Type 0 digit
                // 剩下的都是0了，意思就是只要出现在任何一位，那么这整个数就必然不能是good number了
            }
            else {
                // Step 2，写 >= 10 的 i
                int lastDigit = i % 10;
                int lastDigitType = dp[lastDigit];
                
                // 这题这样从小到大的做法，能确保我们要考察一个数x的时候，x/10一定已经在dp数组里了！
                int prevDigits = i / 10;
                int prevDigitsType = dp[prevDigits];
                
                if (lastDigitType == 0 || prevDigitsType == 0) {
                    dp[i] = 0;
                    continue;
                } 
                if (lastDigitType == 1 && prevDigitsType == 1) {
                    dp[i] = 1;
                    continue;
                }
                // 当前数i里至少有一位是type 2 digit，然后不是type 2 的也一定是 type 1. 没有任何type 0 的
                dp[i] = 2;
                count++;
            }
        }
        return count;
    }
}
```

## Reference
* [Rotated Digits [LeetCode]](https://leetcode.com/problems/rotated-digits/description/)

{% include links.html %}
