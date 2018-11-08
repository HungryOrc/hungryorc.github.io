---
title: "Minimum Cost to Hire K Workers who are of Lowest Qualities, and Pay Them with Highest Rate"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_min_cost_to_hire_k_workers.html
folder: algorithm
toc: false
---

## Description
There are N workers.  The i-th worker has a quality[i] and a minimum wage expectation wage[i].

Now we want to hire exactly K workers to form a paid group.  When hiring a group of K workers, we must pay them according to the following rules:
1. Every worker in the paid group should be paid in the ratio of their quality compared to other workers in the paid group.
2. Every worker in the paid group must be paid at least their minimum wage expectation.
Return the least amount of money needed to form a paid group satisfying the above conditions.

附带说明：
* 1 <= K <= N <= 10000, where N = quality.length = wage.length
* 1 <= quality[i] <= 10000
* 1 <= wage[i] <= 10000
* Answers within 10^-5 of the correct answer will be considered correct.

### Example
* Input: quality = [10,20,5], wage = [70,50,30], K = 2
  * Output: 105.00000
  * Explanation: We pay 70 to 0-th worker and 35 to 2-th worker.
* Input: quality = [3,1,10,10,1], wage = [4,8,2,2,7], K = 3
  * Output: 30.66667
  * Explanation: We pay 4 to 0-th worker, 13.33333 to 2-th and 3-th workers seperately. 

## Solution
首先要明确题意。这一题的意思是很诡异的。首先要招满k个人，无论好赖。第二要给他们一样的工资标准，看齐这k个人里rate最高的那个人。第三这k个人的总quality要最低，就是说要找工作质量最差的k个人，因为rate定了以后，总工资等于rate乘以这k个人的总quality，我们要求最后给的总工资越少越好。

这题的解法有点DP的思想。既然工资是对比某个人的rate来定的，那么每个人都有可能被立为rate的标杆。那么我们每次就假定一个人作为rate标杆。那么所有比他rate低的人都有可能成为他的team里的另外k-1个人，rate比他高的人就不可以进team（当然，如果rate比他低的人不到k-1个，那么就不能用这个人做rate标杆）。那么就要在所有rate比他低的人里面尽快找出quality最低的k-1个人。这里就要用到一个max heap，用类似top k element的思路来做。具体看下面的代码。

在做上面这些之前，就需要算出每个人的 rate = wage / quality，然后用rate从小到大排序所有workers，把他们放到一个数组里。


### Complexity
* Time: O(n * logn)，排序workers数组是 nlogn，后续用heap处理是 nlogk
* Space: O(n)，array of workers

### Java
```java
// helper class
class Worker implements Comparable<Worker> {
    int quality;
    double rate;
    
    public Worker(int quality, double rate) {
        this.quality = quality;
        this.rate = rate;
    }
    
    @Override
    public int compareTo(Worker w2) { // 根据rate从小到大排序
        if (this.rate == w2.rate) {
            return 0;
        }
        return this.rate > w2.rate ? 1 : -1;
    }
}

public class Solution {
    public double mincostToHireWorkers(int[] quality, int[] wage, int k) {
        double result = Double.MAX_VALUE;
        
        Worker[] workers = new Worker[quality.length];
        for (int i = 0; i < quality.length; i++) {
            double rate = wage[i] * 1.0 / quality[i];
            workers[i] = new Worker(quality[i], rate);
        }
        Arrays.sort(workers); // sort by rate, ascending
        
        // 这个heap是max heap，装大家的qualities
        // Collections.reverseOrder() 就是为了实现max heap，没有它就默认是min heap
        // 10 是初始size，它无所谓
        PriorityQueue<Integer> pq = new PriorityQueue<>(10, Collections.reverseOrder());
        int totalQuality = 0;
        for (int i = 0; i < quality.length; i++) {
            // 如果heap里已经有k个人了，那么要移除heap里当前quality最高的那个人，
            // 这样使得heap里剩下的k-1个人是quality最小的k-1个人
            if (pq.size() == k) {
                totalQuality -= pq.poll();
            }
            
            // 现在第i个worker登场了，他的rate一定比当前heap里的所有人的rate都要高，
            // 我们现在正是要用他的rate来pay给所有人，
            // 所以自然他的quality也要算进来，因为他现在是k人小队中的一员
            Worker curWorker = workers[i];
            pq.offer(curWorker.quality);
            totalQuality += curWorker.quality;
            if (pq.size() == k) {
                result = Math.min(curWorker.rate * totalQuality, result);
            }
        }
        return result;
    }
}
```

## Reference
* [Minimum Cost to Hire K Workers [LeetCode]](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/description/)

{% include links.html %}
