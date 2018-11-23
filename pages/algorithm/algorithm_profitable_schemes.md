---
title: "Profitable Schemes"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_profitable_schemes.html
folder: algorithm
toc: false
---

## Description
There are G people in a gang, and a list of various crimes they could commit.

The i-th crime generates a profit[i] and requires group[i] gang members to participate.

If a gang member participates in one crime, that member can't participate in another crime.

Let's call a profitable scheme any subset of these crimes that generates at least P profit, and the total number of gang members participating in that subset of crimes is at most G.

How many schemes can be chosen?  Since the answer may be very large, return it modulo 10^9 + 7. 这是本题的一个特殊要求。和本题的主题逻辑没有任何关系
* 1 <= G <= 100
* 0 <= P <= 100
* 1 <= group[i] <= 100
* 0 <= profit[i] <= 100
* 1 <= group.length = profit.length <= 100

### Example
* Input: G = 5, P = 3, group = [2,2], profit = [2,3]
  * Output: 2
  * To make a profit of at least 3, the gang could either commit crimes 0 and 1, or just crime 1. In total, there are 2 schemes.
* Input: G = 10, P = 5, group = [2,3,5], profit = [6,7,8]
  * Output: 7
  * To make a profit of at least 5, the gang could commit any crimes, as long as they commit one. There are 7 possible schemes: (0), (1), (2), (0,1), (0,2), (1,2), and (0,1,2).

## Solution: DP
这题比较明显是要用DP做，难度是 hard。具体处理有一些需要注意的地方。题意中有2个限制，一个是总人数有上限，一个是总利润有下限，自然想起背包问题，
总重量有上限，总价值有下限那样的题目。
* `int[][] dp = new int[G + 1][allProfitSum + 1]`, 其中 `dp[i][j]` 意思是 正好用了i个人，利润正好是j，一共有多少种方法
* 初始状态：dp[0][0] = 1，即没人也没利润，有一种方式，自然就是不派人也不做事。这个看起来很无厘头，也不好想。但是非常重要！这相当于一生万物的那个一，
没有它，后面的一切都是零，都无从发动
* 递推关系：`dp[memberNumberSum][profitSum] += dp[memberNumberSum - curMemberNumber][profitSum - curProfit]`
  * **这其实是一种 “set of elements” 的思维**。我们把每个 group[i] & profit[i] 看成一个 pair，作为一个整体，一个element。那么我们最后的scheme，就是这些elements的组合。每个element可以被选用1次或0次。
  * 一开始矩阵里只有 0人0利润 是1，其他所有都是0. 这个没问题
  * 假设第一个pair是 4人2利润。那么 4人2利润 即 dp[4][2] 将变成1，它“继承”的是 dp[0][0] 的那个1。然后 dp[0][0]也还是1。其他所有dp cell都还是0。填矩阵到此为止，就表征了这种状况：第一个pair，要么选用，要么不选用
  * 假设第二个pair是 3人5利润。那么 dp[3][5] 将变成1。然后 dp[7][7] 也将变成1。All other zeros stays to be zero。到此为止，就表征了这几种情况：什么都不做:[0][0]=1，只做第一个pair:[4][2]=1，只做第二个pair:[3][5]=1，前两个pair都做:dp[7][7]=1。再往后加入第三个，第四个...pairs的做法同理
* 特别注意，初始状态不要包括 `dp[每个groupMemberNumber][相应的每个profit] = 1`！
  * 这个也不好想，反直觉。习惯来说应该是初始把这一串都列上的。但按照我们的递推公式，就不能把数组 `group` 和 `profit` 里的一对一对的数列在初始条件里。如果那样做，上面的递推式子就会重复！
  * 比如 4人2利润 是一个任务，3人5利润 是另一个任务。那么 7人7利润 应该是有一种方法。但是如果把 4人2利润 和 3人5利润 都列在初始条件里都设为1（种方法），那么 7人7利润 最后就会被加成2种方法。但其实只有一种，即前面两个任务都做才能达成7,7
* **这题最重要的一点。填dp矩阵的时候，要从右下角填到左上角！即两个维度都要从大到小！而非一般习惯的从小到大。** 如果从小到大填，会出现如下问题：
  * 如果数组里第一个pair是 2人3利润，那么 dp[2][3] 会变成 1，这个也没问题。但是就在 2人3利润 的这个pair的for loop里，在晚些时候还会导致 4人6利润 也会变成1！这就不对了。这个是 2人3利润 之前的那个 1 起了作用：loop 到 dp[4][6] 的时候，一看，哎前面 dp[2][3] = 1，那么就把 dp[2+2][3+3] 也变成1了，殊不知前面那个 dp[2][3] 其实就是它自己的作用，而自己的作用不能在一个shceme里起作用两次。这么搞下去，dp[6][8]，dp[8][12]... 都会变成1
  * 如果做到一半，出现这样的情况：dp[5][7] = 5, dp[8][10] = 2，然后我们现在的loop是针对pair 3人3利润。那么合理的结果是，基于 dp[5][7] = 5，会让 dp[5+3][7+3] = dp[8][10] 加上5；基于 dp[8][10] = 2，会让 dp[11][13] 加上2。但是我们如果从小加到大，就会导致 dp[8][10] += 5 变成了 7，因为它原本就等于 2。然后算 dp[11][13] 的时候，dp[11][13] 就会被 += 7，而实际上它应该只被 += 2，多出来的那个5，就是 dp[3][3] 重复作用了两次。
    * 这个就是 “正巧这一脚踩在下一脚的脚印里”的情况，要避免的话比较费事。就算上一个例子里的那种重复被解决，这个要解决也要更复杂
  * 但是如果从大到小加，就不会出现这种 自己踩了自己的脚 的问题！就像你要把一个数组里的所有数都增加相同的或者不同的量的话，从数组尾部开始做，能杜绝踩踏
* 结果：对于dp矩阵里，所有 总profit大于等于P 的列 求和

### Complexity
* Time: O(n)
* Space: O(n)

### Java
```java
class Solution {
    public int profitableSchemes(int G, int P, int[] group, int[] profit) {
        // ignore the corner cases
        
        int n = group.length;
        
        // 这个参数完全是因为题目的要求，和本解法的逻辑无关，要不是数太大，完全可不要此参数
        int magicMod = (int)Math.pow(10, 9) + 7;
        
        int allPrfSum = 0;
        for (int p : profit) { // 认为profit数组里的所有数都是 >= 0 的
            allPrfSum += p;
        }

        int[][] dp = new int[G + 1][allPrfSum + 1];
        dp[0][0] = 1; // 这个初始状态不好想，但很重要   
        
        for (int i = 0; i < n; i++) {
            int mem = group[i];
            int prf = profit[i];

            // 这个做法的灵魂之处！从右下往左上填dp数组！
            for (int m = G; m >= mem; m--) {
                for (int p = allPrfSum; p >= prf; p--) {
                    dp[m][p] = (dp[m][p] + dp[m - mem][p - prf]) % magicMod;
                    // 注意 a = (a + b) % c 和 a += b % c 得到的 a 是不同的
                }
            }
        }
        
        int result = 0;
        for (int i = 1; i <= G; i++) {
            for (int j = P; j <= allPrfSum; j++) {
                result = (result + dp[i][j]) % magicMod;
            }
        }
        return result;
    }
}
```

## Reference
* [Profitable Schemes [LeetCode]](https://leetcode.com/problems/profitable-schemes/description/)

{% include links.html %}
