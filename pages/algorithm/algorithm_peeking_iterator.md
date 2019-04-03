---
title: "Peeking Iterator"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_peeking_iterator.html
folder: algorithm
toc: false
---

## Description
Given an Iterator class interface with methods: next() and hasNext(), design and implement a PeekingIterator that support the peek() operation -- it essentially peek() at the element that will be returned by the next call to next().

这题的关键在于理解peek的作用。peek的作用是，它返回“如果现在做next，会得到什么”的值。但是无论做多少次peek，都没有真的往前走一步！而每一次做next，就一定会往前走一步！

### Follow Up
How would you extend your design to be generic and work with all types, not just integer?

应该是用Generic type就行

### Example
Assume that the iterator is initialized to the beginning of the list: [1,2,3].

Call next() gets you 1, the first element in the list.
Now you call peek() and it returns 2, the next element. Calling next() after that still return 2. 
You call next() the final time and it returns 3, the last element. 
Calling hasNext() after that should return false.

## Solution
Java Iterator interface reference:
https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

### Complexity
* Time: O(1)
* Space: O(1)

### Java
```java
class PeekingIterator implements Iterator<Integer> {
    Integer cache = null;
    Iterator<Integer> it;
    
	public PeekingIterator(Iterator<Integer> iterator) {
	    this.it = iterator;
	    cache = it.next();
	}

  // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return cache;
	}

	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    int ret = cache;
	    if(it.hasNext()){
	        cache = it.next();
	    }
	    else{
	        cache = null;
	    }
	    return ret;
	}

	@Override
	public boolean hasNext() {
	    return (cache != null);
	}
}
```

## Reference
* [Peeking Iterator [LeetCode]](https://leetcode.com/problems/peeking-iterator/description/)

{% include links.html %}
