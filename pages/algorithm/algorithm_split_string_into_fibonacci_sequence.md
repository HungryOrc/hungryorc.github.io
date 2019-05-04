---
title: "Split a String (of Digits) into Fibonacci Sequence"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_split_string_into_fibonacci_sequence.html
folder: algorithm
toc: false
---

## Description
Given a string S of digits, such as S = "123456579", we can split it into a Fibonacci-like sequence [123, 456, 579].
* S contains only digits, no other kinds of char
* The length of S belongs to range [1, 200]

Formally, a Fibonacci-like sequence is a list F of non-negative integers such that:
* 0 <= F[i] <= 2^31 - 1, (that is, each integer fits a 32-bit signed integer type);
* F.length >= 3;
* and F[i] + F[i+1] = F[i+2] for all 0 <= i < F.length - 2.

注意：
* 没有非法字符。只有0-9。也没有负号
* when splitting the string into pieces, **each piece must not have extra leading zeroes, except if the piece is the number 0 itself**，这个含“零”的规则要注意
* 不能把几个0放到一起，比如00或者000是不行的。拆成几个连续的单个0是可以的，因为 0 + 0 = 0 也是ok的
* **给的String的长度可能会超过int型变量的上限**(10位十进制数)，但**每个切分下来的数不能超过int型上限**，也就是说初始的2个数，以及后面各个被加出来的数，都不可以超过10位十进制数（而且如果是十位，它的首位也不能超过2）。这个在我们做切分判断的时候要格外注意

Return any Fibonacci-like sequence split from S, or return [] if it cannot be done.

### Example
* Input: "11235813"
  * Output: [1,1,2,3,5,8,13]
* Input: "123456579"
  * Output: [123,456,579]
* Input: "112358130"
  * Output: [], 无法划分
* Input: "0123"
  * Output: []
  * Leading zeroes are not allowed, so "01", "2", "3" is not valid
* Input: "1101111"
  * Output: [11, 0, 11, 11]，或者 [110, 1, 111]
  * 注意，中间有0是有可能ok的，leading zero 而且不止一位的话是不行的

## Solution 1: Backtracking，速度较快，前10%
Ref: https://leetcode.com/problems/split-array-into-fibonacci-sequence/discuss/133936/short-and-fast-backtracking-solution

这种斐波那契型的数列，一旦前两个定了，则后面的就都唯一确定了，无法更改无法阻挡。
那么中间任何一处续不上去，就意味着此路不通。

### Complexity
* Time: O(n^3)
* Space: O(n^2)

### Java
```java
class Solution {
    public List<Integer> splitIntoFibonacci(String S) {
        // ignore sanity checks
        List<Integer> result = new ArrayList<>();
        helper(S, result, 0);
        return result;
    }

    public boolean helper(String str, List<Integer> result, int idx) {
        if (idx == str.length() && result.size() >= 3) {
            return true;
        }
        
        for (int i = idx; i < str.length(); i++) {
            // 开头为0且长度大于1的数，不要
            if (str.charAt(idx) == '0' && i > idx) {
                break;
            }
            
            long num = Long.parseLong(str.substring(idx, i + 1));
            
            if (num > Integer.MAX_VALUE) {
                break;
            }
            
            int size = result.size();

            // 如果当前尝试的序列里已经有2个或更多的数字，而且现在接龙搞不下去了，
            // 则放弃这个序列
            if (size >= 2 && num > result.get(size - 1) + result.get(size - 2)) {
                break;
            }
            
            // 如果当前尝试的序列里还不到2个数字，或者序列里到目前为止接龙还ok，
            // 那么就继续干
            if (size < 2 || num == result.get(size - 1) + result.get(size - 2)) {
                result.add((int)num);
                
                // 如果任何一个分支ok了，就把true往上一层传递！作用是结束上面的每一层！
                // 后面的复原也不用做了
                if (helper(str, result, i + 1)) {
                    return true;
                }
                
                result.remove(result.size() - 1); // 复原
            }
        }
        return false;
    }
}
```

## Solution 2: 我的DP解法，这一题DP反而不如backtracking快
* 这题有点DP的范儿，也确实可以用DP类型的思路来做，但具体落实到做法其实不算是真正的DP
* 为了快速得到任何起始位置任何终止位置的数的值，可以做一个辅助的二维数组，先把所有数都算好存在这里
  * 算这个数组的时候也有技巧，为了不用O(n)的时间长度，可以也用DP的思路来算各个分段的值：一个长度为m的分段的值等于前m-1位的值乘以10加上第m位的值

### Complexity
* Time: O(n^3)
* Space: O(n^2)

### Java
除去2个helper functions之后，主函数逻辑其实也不很长
```java
class Solution {
    public List<Integer> splitIntoFibonacci(String S) {
        if (S == null || S.length() == 0) {
            return new ArrayList<Integer>();
        }
        
        int n = S.length();
        
        // 填写values矩阵
        int[][] values = new int[n][n];
        for (int i = 0; i < n; i++) {
            values[i][i] = S.charAt(i) - '0';
        }
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i + len - 1 < n; i++) {
                int end = i + len - 1;
                if (values[i][i] == 0) {
                    values[i][end] = -1;
                    continue;
                }
                values[i][end] = values[i][end - 1] * 10 + values[end][end];
            }
        }
        
        // 根据前两个数，往后不断加，加到哪里不对了就false，加到S的尾部都一直ok就true
        
        int start1 = 0;
        // < n - 2 是因为num2和num3都至少长度为1，当然这里写 < n 也不会出错
        for (int len1 = 1; start1 + len1 - 1 < n - 2; len1++) {
            int end1 = start1 + len1 - 1;
            int start2 = end1 + 1;
            int initialEnd1 = end1;
            
            // 加2次len2是因为len3也存在，且len3>=len2
            for (int len2 = 1; start2 + len2 - 1 + len2 - 1 < n; len2++) {
                int end2 = start2 + len2 - 1;
                int initialEnd2 = end2;
                
                int num1 = values[start1][end1];
                int num2 = values[start2][end2];
                
                // 这是为了避开那些开头是0又不止一位的数
                if (num1 == -1 || num2 == -1) {
                    continue;
                }
                
                int num3 = num1 + num2;
                int len3 = getNumDigits(num3);
                int start3 = end2 + 1;
                int end3 = start3 + len3 - 1;
                
                int sumLen = len1 + len2;
                
                while (end3 < n && sumLen < n) {
                    if (values[start3][end3] != num3) {
                        break;
                    }
                    
                    num1 = num2;
                    num2 = num3;
                    num3 = num1 + num2;
                    
                    sumLen += len3;
                    
                    start3 = end3 + 1;
                    len3 = getNumDigits(num3);
                    end3 = start3 + len3 - 1;
                }
                
                if (sumLen == n) {
                    return getResult(n, values[0][initialEnd1], values[initialEnd1 + 1][initialEnd2]);
                }
                // if sumLen>n or sumLen<n, do nothing
            }
        }
        
        
        return new ArrayList<>();
    }
    
    private List<Integer> getResult(int n, int num1, int num2) {
        List<Integer> result = new ArrayList<>();
        result.add(num1);
        result.add(num2);
        
        int lenSum = getNumDigits(num1) + getNumDigits(num2);
        int num3 = num1 + num2;
        
        while (lenSum < n) {
            result.add(num3);
            lenSum += getNumDigits(num3);
            
            num1 = num2;
            num2 = num3;
            num3 = num1 + num2;
        }
        return result;
    }
    
    // 返回一个整数有几位数
    private int getNumDigits(int num) {
        // log_10(0) 等于负无穷，所以不能用log来求0的位数
        if (num == 0) {
            return 1;
        }
        return (int)(Math.log10(num) + 1);
    }
}
```

## Reference
* [Split Array into Fibonacci Sequence [LeetCode]](https://leetcode.com/problems/split-array-into-fibonacci-sequence/description/)

{% include links.html %}
