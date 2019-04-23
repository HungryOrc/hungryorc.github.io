---
title: "Swap a Pair of Chars to Make Two Strings Equal"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_swap_a_pair_of_chars_to_make_two_strings_equal.html
folder: algorithm
toc: false
---

## Description：Google 电面题
给两个string s1, s2. 将s1中的两个char交换一次，且只能交换一次，得到一个string，是否等于s2
* 如果两个string相同，return false; 因为不能交换
* 如果有一个char不同 return false; 因为不能交换。 
* 如果有两个char不同，return 两个char换个位置是否等于s2。 
* 如果有超过两个char不同，return false 因为一次交换肯定不能变成s2。 

### Example
* Input: "aa" and "aa"
  * Output: False
* Input: "abc" and "bca"
  * Output: False
* Input: "abc" and "cba"
  * Output: True
* Input: "xaxbxax" and "xbxaxbx"
  * Output: False
  
## Solution
要细心。有些小坑

### Complexity
* Time: O(n)
* Space: O(1)

### Java
代码虽然看起来不短，其实逻辑很简单
```java
public class Solution {
    public boolean swapToEqual(String s1, String s2) {
        if (s1 == null || s2 == null) {
            return false;
        }
        if (s1.length() != s2.length()) {
            return false;
        }

        int n = s1.length();
        Set<Character> set = new HashSet<>();
        int countOfDiffIndex = 0;
        
        // 两个string所差异的chars必须是2个，不能多不能少，
        // 而且必须出现在2个相对应的index上！比如s1上是index 3和7，那么s2上也必须是3和7！
        // 而且也不能出现在3个相对应的index上！比如index 3，7，10都有不同的话也不行，就算还是那2个chars
        for (int i = 0; i < n; i++) {
            char c1 = s1.charAt(i);
            char c2 = s2.charAt(i);
            
            if (c1 != c2) {
                if (set.size() == 0) {
                    set.add(c1);
                    set.add(c2);
                    countOfDiffIndex = 1;
                    continue;
                }
                
                if (!set.contains(c1) || !set.contains(c2)) { // 如果出现了第三种不同的char
                    return false;
                }
                
                countOfDiffIndex++;
                if (countOfDiffIndex > 2) { // 如果2种不同的chars出现了超过2次
                    return false;
                }
            }
        }
        
        if (set.size() == 0) { // 如果两个string完全相同，也是不行的
            return false;
        }
        return true;
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
