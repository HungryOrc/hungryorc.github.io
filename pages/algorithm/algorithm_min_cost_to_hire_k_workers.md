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
哦也

### Complexity
* Time: O(n*logn)，排序workers数组是n*logn，后续用heap处理是n*logk
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
