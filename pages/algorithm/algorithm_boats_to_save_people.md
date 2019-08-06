---
title: "Boats to Save People"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_boats_to_save_people.html
folder: algorithm
toc: false
---

## Description
The i-th person has weight people[i], and each boat can carry a maximum weight of limit.

Each boat carries at most 2 people at the same time, provided the sum of the weight of those people is at most limit.

Return the minimum number of boats to carry every given person.  (It is guaranteed each person can be carried by a boat.)

Note:
* 1 <= people.length <= 50000
* 1 <= people[i] <= limit <= 30000

注意：
* 一条船上最多坐2个人
* 最重的人也一定会比limit轻，因为题目保证所有人都能被船救走

### Example
* Input: people = [1,2], limit = 3
  * Output: 1 boat, (1, 2)
* Input: people = [3,2,2,1], limit = 3
  * Output: 3 boats, (1, 2), (2) and (3)
* Input: people = [3,5,3,4], limit = 5
  * Output: 4 boats, (3), (3), (4), (5)

## Solution 1：排序，然后左右指针向中间靠拢。速度 前5%
能这么做的原因有2点：首先，每条船最多坐2个人，第二，每条船在任何情况下都能至少坐进去一个人，即所有人的体重都小于船的载重limit

### Complexity
* Time: O(n logn)，排序所需的时间
* Space: O(1)

### Java
```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int n = people.length;
        
        int count = 0;
        int left = 0, right = n - 1;
        
        while (left < right) {
            if (people[right] + people[left] > limit) {
                count++;
                right--;
                continue;
            }
            
            count++;
            right--;
            left++;
        }
        
        // 如果还剩最后一个人（在中间的某个位置，left和right重合之处），此时即 left == right
        // 还有一种情况就是 到了此时 right + 1 == left
        if (left == right) return count + 1;
        return count;
    }
}
```


## Solution 2：用bucket sort 来实现 O(n)的速度。不好想到，仅供参考就行
原贴在此：https://leetcode.com/problems/boats-to-save-people/discuss/197063/easy-thought-process-to-improve-from-O(nlogn)-to-O(n)

### Complexity
* Time: O(n)
* Space: O(1)

### Java
```java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        int[] buckets = new int[limit + 1];
        for (int p: people) buckets[p]++;
        
        int start = 1;
        int end = limit;
        int solution = 0;
        
        while (start <= end) {
            // make sure the start always point to a valid number
            while (start <= end && buckets[start] == 0) start++;
            
            // make sure end always point to valid number
            while (start <= end && buckets[end] == 0) end--;
            
            // 这个意味着所有人都上船了已经，这个不好理解，要多想一下才行
            if(buckets[start] <= 0 && buckets[end] <= 0) break;
            
            solution++;
            
            if (start + end <= limit) buckets[start]--; // both start and end can carry on the boat
            buckets[end]--;
        }
        return solution;
    }
}
```

## Reference
* [Boats to Save People [LeetCode]](https://leetcode.com/problems/boats-to-save-people/description/)

{% include links.html %}
