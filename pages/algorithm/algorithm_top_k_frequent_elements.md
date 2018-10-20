---
title: "Top K Frequent Elements in an Array"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_top_k_frequent_elements.html
folder: algorithm
toc: false
---

## Description
Given a non-empty array of integers, return the k most frequent elements.
* You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
* Your algorithm's time complexity must be better than O(n logn), where n is the array's size.

### Example
* Input: nums = [1,1,1,2,2,3], k = 2
  * Output: [1,2]

## Solution 1: HashMap + MinHeap
用 hashmap 来装各个string的出现次数，然后关键在于用 Min Heap 来处理最高频的 k 个 Map Entry

### Complexity
* Time: O(n logn)
* Space: O(n)

### Java
```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> counts = new HashMap<>();
        for (int n : nums) {
          counts.put(n, counts.getOrDefault(n, 0) + 1);
        }

        PriorityQueue<Map.Entry<Integer, Integer>> minHeap = 
          new PriorityQueue(k, new Comparator<Map.Entry<Integer, Integer>>() {
          @Override
          public int compare(Map.Entry<Integer, Integer> entry1, Map.Entry<Integer, Integer> entry2) {
            return entry1.getValue().compareTo(entry2.getValue());
          }
        });

        // put the map entries into the min heap
        // if the total number of map entries is < k, it's still ok
        for (Map.Entry<Integer, Integer> entry : counts.entrySet()) {
          if (minHeap.size() < k) {
            minHeap.offer(entry);
          } else {
            if (entry.getValue() > minHeap.peek().getValue()) {
              minHeap.poll();
              minHeap.offer(entry);
            }
          }
        }

        List<Integer> result = new LinkedList<>();
        while (!minHeap.isEmpty()) {
          result.add(minHeap.poll().getKey()); // 注意这里要get key！不要getValue
        }
        return result;
    }
}
```

## Solution 2: HashMap + TreeMap 高大上！但速度和MinHeap差不多
Use freqncy as the key so we can get all freqencies in order

### Complexity
* Time: O(n logn)
* Space: O(n)

### Java
```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        // <number, frequency>
        Map<Integer, Integer> counts = new HashMap<>();
        for(int n: nums){
            counts.put(n, counts.getOrDefault(n, 0) + 1);
        }

        // <frequency , list of numbers that appeared at this frequency>
        TreeMap<Integer, List<Integer>> freqMap = new TreeMap<>();
        for(int num : counts.keySet()){
            int freq = counts.get(num);
        
            List<Integer> numsAtThisFreq = freqMap.get(freq);
            if(numsAtThisFreq == null){
                numsAtThisFreq = new LinkedList<Integer>();
                freqMap.put(freq, numsAtThisFreq);
            }
            numsAtThisFreq.add(num);
        }

        List<Integer> result = new LinkedList<>();
        while(result.size() < k){
            Map.Entry<Integer, List<Integer>> entry = freqMap.pollLastEntry();
            result.addAll(entry.getValue());
        }
        return result;
    }
}
```

## Solution 3: 我的 Bucket Sort 型土办法，但似乎比前两个还快一点点！
做一个长度为n+1的数组。如果一个数x出现的次数为m，就在这个数组的第m个index上存下x

但还有可能另一个数y也出现了m次，也需要记在index = m的位置，所以这个数组，其中的每个元素都得是一个ArrayList

记录完以后，从这个数组的尾部开始往前捋。捋满了k个数，或者捋到了数组的头部以后，即得到答案
    
### Complexity
* Time: O(n)   《===== 对么？？？
* Space: O(n)

### Java
```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> numCountsMap = new HashMap<>();
        for (int n : nums) {
            numCountsMap.put(n, numCountsMap.getOrDefault(n, 0) + 1);
        }
        
        int n = nums.length;
        ArrayList<Integer>[] indexIsCount = new ArrayList[n+1];
        for (int i = 0; i < n+1; i++) {
            ArrayList<Integer> temp = new ArrayList<>();
            indexIsCount[i] = temp;
        }
        
        for (Map.Entry<Integer,Integer> curEntry : numCountsMap.entrySet()) {
            int curNum = curEntry.getKey();
            int curCount = curEntry.getValue();
            
            indexIsCount[curCount].add(curNum);
        }
        
        ArrayList<Integer> result = new ArrayList<>();
        int stillNeedToPick = k;
        for (int i = n; i >= 0 && stillNeedToPick > 0; i--) {
            if (indexIsCount[i].size() > 0) {
                for (int j = 0; j < indexIsCount[i].size() && stillNeedToPick > 0; j++) {
                    result.add(indexIsCount[i].get(j));
                    stillNeedToPick --;
                }
            }
        }
        return result;
    }
}
```

## Reference
* [Top K Frequent Elements [LeetCode]](https://leetcode.com/problems/top-k-frequent-elements/description/)

{% include links.html %}
