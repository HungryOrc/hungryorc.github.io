---
title: "Election Winner"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_election_winner.html
folder: algorithm
toc: false
---

## Description
输入一个String数组，里面是n个投票者对于各个候选人的投票记录。数组里的每个元素是一个候选人的名字，比如：Tom, Jerry, Donald, Micky, Donald, Tom, Tom...

要求得票最多的候选人的名字。如果有并列第一（2个或多个并列第一），则取字母顺序 靠 最 后 的那个

### Example
略

## Solution：一次过
把数组走一遍。在过程里维护一个map，并实时更新 max vote number，并实时比较并列第一的人的名字！这样就不必搞完map以后再从头到尾捋一遍了

### Complexity
* Time: O(n)
* Space: O(n), hashmap

### Java
```java
class Solution {
    public String electionWinner(String[] votes) {
        // ignore validations
        
        // <candidate name, num of votes to this candidate>
        Map<String, Integer> map = new HashMap<>();
        String winner = "";
        int maxVote = 0;
        
        for (String name : votes) {
            int count = map.getOrDefault(name, 0);
            count++;
            map.put(name, count);
            
            // > 和 == 这两种情况 一定要分开处理
            if (count > maxVote) {
                maxVote = count;
                winner = name;
            } else if (count == maxVote) {
                // 注意这个 两个 String 之间的比较
                if (name.compareTo(winner) > 0) {
                    winner = name;
                }
            }
        }
        return winner;
    }
}
```

## Reference
这题应该是在 HackerRank 上

{% include links.html %}
