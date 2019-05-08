---
title: "Max Min Gap of K Elements in An Array"
tags: [algorithm, binary_search]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_max_min_gap_of_k_elements_in_array.html
folder: algorithm
toc: false
---

## Description
一个int array，里面的元素都是正整数，从小到大排列好的。每两个element之间的距离 定义为 它们两的值之差。那么k个element之间的最短间距，就是看这k个里面逐个的距离哪个最小了。
问：在array里随便找k个元素，它们的最短间距的最大值是多少

### 我自己想到的 Follow up
如果k个点是在一个平面上，而非直线上，那怎么快速计算最短间距？

还没想到好的办法。难道用geo hash？分为多个小方块来算最短间距？

### Example
* Input: [2, 3, 4, 7], k = 3
  * Output: 2。这(k=)3个点分别取在 2，4，7，这是它们可能实现的两两之间隔得最开的方案。所以最大的的最小间距是2

## Solution：高级的二分法运用
相当于在数组里随便pick k 个点，这k个点两两之间要尽量隔开得越远越好。那么自然数组的两个端点是要选的。然后中间再选 k - 2 个点。
到了这里就和锯木头的那题基本一样了：**一个长木头上有一些分段标记，必须锯在这些分段标记上**。

关键点：**用二分法来不断接近和摸索最小间距的最大值**

### Complexity
* Time: O(nlogn) <=== ？？？
* Space: O(n)，worst case of call stacks <=== ？？？

### Java
代码对么 ？？？
```java
public class Solution {
    public int maxMinDist(int[] nums, int k) {
        if (nums == null || nums.length == 0 || k <= 1) {
            return 0;
        }
        int n = nums.length;
        if (k > n) {
            return 0;
        }
        if (k == 2) {
            return nums[n - 1] - nums[0];
        }
        
        int maxDist =  nums[n - 1] - nums[0];
        int minDist = Integer.MAX_VALUE;
        for (int i = 0; i < n - 1; i++) {
            minDist = Math.min(minDist, nums[i + 1] - nums[i]);
        }
        
        int[] maxMin = {0};
        getMaxMin(nums, k, minDist, maxDist, maxMin);
        return maxMin[0];
    }
    
    private void getMaxMin(int[] nums, int k, int low, int high, int[] maxMin) {
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int[] actualMin = {Integer.MAX_VALUE};
            int cuts = getNumOfCuts(nums, mid, actualMin);
            
            // 如果切出来的块多于k，那么就需要切割间距加大，
            if (cuts > k) {
                low = mid + 1;
            // 要特别注意！如果切出来的块正好等于k，也要再把切割间距缩小试试看！看能否还是正好切出k块！！
            // 间距之所以要变小，而非变大，是因为一共有k个元素被选出来的话，很可能最后一个是不足长度的，
            // 那么这个最后一节所导致的 min sit 就会很小
            // 那么把前面的那些段都缩小的话，可能就会把最后一个扩大，这样总体来说就是加大了 min dist！！
            } else if (cuts == k) {
                high = mid - 1;
                // 只有在切割数正好等于k的时候，此时的actual min 间隔值 才有资格和 maxMin 相比较！
                maxMin[0] = Math.max(maxMin[0], actualMin[0]);
            } else {
                high = mid - 1;
            }
        }
    }
    
    // 这里的参数len的意思是：切出来的每一段的长度都要 >= len
    private int getNumOfCuts(int[] nums, int len, int[] actualMin) {
        int count = 1; // 一开始就是1，意思是最左端的那个点也是“切了一下”
        int curLen = 0;
        
        for (int num : nums) {
            if (curLen < len) {
                curLen += num;
            } else {
                actualMin[0] = Math.min(actualMin[0], curLen);
                count++;
                curLen = 0;
            }
        }
        
        if (curLen < len) { // 如果最后一刀没有正好切到数组的右端点上
            // 最后一段很可能会小于len！
            actualMin[0] = Math.min(actualMin[0], curLen);
            count++;
        }
        return count;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
