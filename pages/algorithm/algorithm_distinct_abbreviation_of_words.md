---
title: "Word Abbreviation"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_distinct_abbreviation_of_words.html
folder: algorithm
toc: false
---

## Description: Hard
Given an array of n distinct non-empty strings, you need to generate minimal possible abbreviations for every word following rules below.
* Begin with the first character and then the number of characters abbreviated, which followed by the last character.
* If there are any conflict, that is more than one words share the same abbreviation, a longer prefix is used instead of only the first character until making the map from word to abbreviation become unique. In other words, a final abbreviation cannot map to more than one original words.
* If the abbreviation doesn't make the word shorter, then keep it as original.

Note:
* Both n and the length of each word will not exceed 400.
* The length of each word is greater than 1.
* The words consist of lowercase English letters only.
* The return answers should be in the same order as the original array.

### Example
* Input: `["like", "god", "internal", "me", "internet", "interval", "intension", "face", "intrusion"]`
  * Output: `["l2e","god","internal","me","i6t","interval","inte4n","f2e","intr4n"]`

## Solution: 我的方法，速度 前5%
这题的思路其实是比较符合直觉的，只是想的过程里，要考虑到一些特殊情况，写的过程也要细心，就像拿着一根狼毫为一粒米上色

注意（能想到下面这些，只能用一个字来形容我了：太厉害了）：
* 缩减的数字可以是多位的，比如 `abc250t` 和 `ab251t`
* 完全有可能，缩减的数字变了，而总的缩写的长度不变，比如 `ab100t` 和 `abc99t`

### Complexity
* Time: O(n^2 * m)，n 是words的个数，m 是每个词的平均长度
  * 最差的情况，可能每个词都要和每个之前的词撞车一次，那么一共要撞车 1 + 2 + 3 + ... + (n-1) 次 <==== ？？？？
* Space: O(n * m)，我们用到的map的size <==== ？？？？

### Java
```java
class Solution {
    public List<String> wordsAbbreviation(List<String> dict) {
        if (dict == null || dict.size() == 0) {
            return dict;
        }
        
        int n = dict.size();
        String[] result = new String[n];
        
        // <original word, index of this word in the original list>
        Map<String, Integer> indexes = new HashMap<>();
        
        // <abbreviation of this word, original word>
        Map<String, String> abbrs = new HashMap<>();
        
        for (int i = 0; i < n; i++) {
            String word = dict.get(i);
            indexes.put(word, i);
            
            if (word.length() <= 3) {
                result[i] = word;
            }
            
            getAbbr(word, 1, indexes, abbrs, result);
        }
        
        List<String> resultList = new ArrayList<>();
        for (String s : result) resultList.add(s);
        return resultList;
    }
    
    private void getAbbr(String word, int lenOfPrefix, // 如果缩写是 abc3e，那么 prefix 就是 abc
            Map<String, Integer> indexes, Map<String, String> abbrs, String[] result) {
        int indexForCurWord = indexes.get(word);
        
        if (lenOfPrefix + 2 >= word.length()) {
            result[indexForCurWord] = word;
            return;
        }
        
        String abbr = abbreviate(lenOfPrefix, word);
        
        if (!abbrs.containsKey(abbr)) {
            abbrs.put(abbr, word);
            result[indexForCurWord] = abbr;
            return;
        }
        
        if (abbrs.containsKey(abbr)) {
            getAbbr(word, lenOfPrefix + 1, indexes, abbrs, result);
            
            String conflictWord = abbrs.get(abbr);
            abbrs.put(abbr, null); // 对于发生冲突的位置，我们都必须做这个处理
            
            if (conflictWord == null) return;
            
            getAbbr(conflictWord, lenOfPrefix + 1, indexes, abbrs, result);
        }
    }
    
    private String abbreviate(int lenOfPrefix, String word) {
        String prefix = word.substring(0, lenOfPrefix);
        return prefix + (word.length() - lenOfPrefix - 1) + word.charAt(word.length() - 1);
    }
}
```

## Reference
* [Word Abbreviation [LeetCode]](https://leetcode.com/problems/word-abbreviation/description/)

{% include links.html %}
