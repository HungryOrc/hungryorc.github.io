---
title: "Employee Importance"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_employee_importance.html
folder: algorithm
toc: false
---

## Description
You are given a data structure of employee information, which includes the employee's unique id, his importance value and his direct subordinates' id.

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is not direct.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all his subordinates.[[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
```java
class Employee {
    // unique id of this employee
    public int id;
    // the importance value of this employee
    public int importance;
    // the id of this employee's direct subordinates
    public List<Integer> subordinates;
}
```

Note:
* One employee has at most one direct leader and may have several subordinates.
* The maximum number of employees won't exceed 2000.

### Example
* Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
  * Output: 11. Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.

## Solution: 直接用 BFS 即可

### Complexity
* Time: O(n) <==== ？？？？
* Space: O(n)

### Java
```java
class Solution {
    public int getImportance(List<Employee> employees, int id) {
        // <id, employee>
        Map<Integer, Employee> map = new HashMap<>();
        
        for (Employee eply : employees) {
            map.put(eply.id, eply);
        }
        
        int sum = 0;
        Set<Integer> visited = new HashSet<>();
        visited.add(id);
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(id);
        
        while (!queue.isEmpty()) {
            int curId = queue.poll();
            Employee curEmp = map.get(curId);
            
            sum += curEmp.importance;
            
            for (int sub : curEmp.subordinates) {
                if (visited.contains(sub)) continue;
                
                visited.add(sub);
                queue.offer(sub);
            }
        }
        return sum;
    }
}
```

## Reference
* [Employee Importance [LeetCode]](https://leetcode.com/problems/employee-importance/description/)

{% include links.html %}
