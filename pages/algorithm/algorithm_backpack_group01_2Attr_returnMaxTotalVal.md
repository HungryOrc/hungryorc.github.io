---
title: "Backpack: Group 0 or 1, Two Attributes, Limited Total Size, Return Max Total Value"
tags: [algorithm, dynamic_programming]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_backpack_group01_2Attr_returnMaxTotalVal.html
folder: algorithm
toc: false
---

## Description
Group 0/1 是指：每个group里的items 最多取 1个1次，最少取 0次。一个group里可能有 1个 或 多个items。

每个item有2个属性，size和value。背包有最大的 size 容量。求最大的总 value。背包不必被填满。

### Example
略

## Solution 1：二维DP <=== 对么 ？？？？
* `int dp[i][s]`：使用数组里 index为0到i 的 groups 中的 0个or任意个 groups，
每个被选中的group里 任选1个item 1次，
正好组成 总size = s，最大可能的 总value 是多少。
* `int dp[][] = new int[number of groups][capacity of backpack + 1]`
* Base Cases
  * 对于第一个group，对于它里面的每个item，最多只有一个item可以被选用，最多只能被选用一次，所以:
    * for each item in group 0, if itemSize <= capacity, `dp[0][itemSize] = max(itemValue)`
    * Attention! In each group, **there might be items of same size but different values**! And this might also happen across different groups!
  * `dp[i][0] = 0`，因为总size为0的话，总value自然为0。既然是0也就不用写出来了
* Induction Rule
  * 注意，这一题的递推公式非常特别！每一次 要添入一个新的item 到组合里去的时候，要把 所有存在的items全都试一遍！Check it in the code below
* Return: 注意，这里不是返回 `dp[n - 1][capacity]`。因为获得总value最大时，总size未必是 capacity。所以：
  * Return max(dp[n - 1][1 <= totalSize <= capacity])

### Complexity
* Time: O(n * m * capacity), 其中n是groups的个数，m是平均每个group里面的items的个数
* Space: O(n * capacity)。可以优化为 O(capacity)，因为dp矩阵里，第i行永远只用到第i-1行

### Java
```java
public class Solution {

    // In the array named "sizes", each element is a list of Integer,
    // each list represents a group, the elements in each list represent the sizes of the items in that group.
    // Same for the array named "values"
    public int backPack_unkownProblemNumber(int capacity, List<Integer>[] sizes, List<Integer>[] values) {
        if (capacity <= 0 || sizes == null || values == null ||
            sizes.length == 0 || values.length == 0 || sizes.length != values.length) {
            return 0;        
        }
        // 这里还考虑了sizes数组和values数组长度不同的情况！
        
        int n = sizes.length; // number of groups, not items
        int[][] dp = new int[n][capacity + 1];
        
        // base case 1, for group 0
        List<Integer> sizesForGroup0 = sizes[0];
        List<Integer> valuesForGroup0 = values.get[0];

        for (int i = 0; i < sizesForGroup0.size(); i++) {
            int curSize = sizesForGroup0.get(i);
            int curValue = valuesForGroup0.get(i);

            // Always use "Math.max" here!!
            dp[0][curSize] = Math.max(curValue, dp[0][curSize]);
        }

        for (int totalSize = 1; totalSize <= capacity; totalSize++) {
            // start from the 2nd group
            for (int i = 1; i < n; i++) {
                List<Integer> sizeList = sizes[i];
                List<Integer> valueList = values.get[i];

                // Always use "Math.max" here!! Since we might drop by this totalSize for 
                // multiple times within one group!!
                dp[i][totalSize] = Math.max(dp[i][totalSize], dp[i - 1][totalSize]); 
                    
                for (int j = 0; j < sizeList.size(); j++) { // for each item in this group
                    int curItemSize = sizeList.get(j);
                    int curItemValue = valueList.get(j);

                    if (totalSize >= curItemSize) {
                        dp[i][totalSize] = Math.max(
                            dp[i][totalSize], 
                            dp[i - 1][totalSize - curItemSize] + curItemValue);
                    }
                }
            }
        }
        
        int maxTotalValue = 0;
        for (int sum = 1; sum <= capacity; sum++) {
            maxTotalValue = Math.max(maxTotalValue, dp[n - 1][sum]);
        }
        return maxTotalValue;
    }
}
```

## Solution 1.1：基于Solution 1，使用Offset One方法，简化代码
所谓的Offset One方法，就是：dp[i][j] 里的 i 原本意思是 index为i的元素，现在意思是 **第i个** 元素。这样设置以后，对有些题目，代码能简化不少，对有些题目作用不明显

Offset One方法不能降低时间和空间复杂度

### Complexity
* Time: 与Solution 1 相同
* Space: 与Solution 1 相同

### Java
```java

```

## Solution 1.2：基于Solution 1，dp[index][size]降维为dp[size]

### 只要是DP矩阵降维，就要考虑从大往小填写（矩阵里的一个或多个维度）！

### Complexity
* Time: 与Solution 1 相同
* Space: O(capacity)

### Java
```java

```

## Solution 2：DFS。is there a way to do this question via DFS ????

### Complexity
* Time: O(2^n) <=== 对么 ？？？？
* Space: O(n)，因为有n层call stack <=== 对么 ？？？？

### Java
the following code is wrong! not for this question! just for reference!!!

```java
public class Solution {
    public int backPackII(int capacity, int[] sizes, int[] values) {
        if (sizes == null || sizes.length == 0 || values == null || values.length == 0 ||
            sizes.length != values.length || capacity <= 0) {
            return 0;
        }
        
        return maxTotalVal(sizes, values, 0, capacity, 0);
    }
    
    private int maxTotalVal(int[] sizes, int[] values, int curIndex, 
            int remainSize, int curTotalVal) {
        // 这个条件要写在第一个
        if (remainSize < 0) { 
            return 0; // 失败
        }
        
        if (remainSize == 0 || curIndex == sizes.length) { 
            return curTotalVal;
        }
        
        int curSize = sizes[curIndex];
        int curVal = values[curIndex];
        
        int totalVal = 0;
        // 不使用 current item
        totalVal = Math.max(totalVal,
                            maxTotalVal(sizes, values, curIndex + 1, 
                                        remainSize, curTotalVal));
        // 使用 current item
        totalVal = Math.max(totalVal,
                            maxTotalVal(sizes, values, curIndex + 1, 
                                        remainSize - curSize, curTotalVal + curVal)); 
        return totalVal;
    } 
}
```

## Reference
Hasn't found this question online

{% include links.html %}
