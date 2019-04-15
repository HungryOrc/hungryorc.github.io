---
title: "Median of Stream of Integers"
tags: [algorithm, sliding_window]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_median_of_stream_of_integers.html
folder: algorithm
toc: false
---

## Description (Hard)
Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,
* [2,3,4], the median is 3
* [2,3], the median is (2 + 3) / 2 = 2.5

Design a data structure that supports the following two operations:
* `void addNum(int num)` - Add a integer number from the data stream to the data structure.
* `double findMedian()` - Return the median of all elements so far.

Your MedianFinder object will be instantiated and called as such:
```java
MedianFinder obj = new MedianFinder();
obj.addNum(num);
double param_2 = obj.findMedian();

// for example
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

### Follow Up
* If all integer numbers from the stream are between 0 and 100, how would you optimize it?
* If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?

### Example
见上文

## Solution 1：一个min heap 一个max heap，速度 前15%
用2个Heap，一个min heap 来维持较大的一半的数，一个max heap 来维持较小的一半的数。

然后我们要维持2个heap的size的大小，做到 max heap 的size 要么等于 min heap 的size，要么比min heap 的size 大一。具体如下：
来了一个新的数value的时候：

(1) 如果此时max heap size == min heap size
if max heap 为空（即 min heap 也为空），则把 value 放到 max heap里去
else if value的值 小于等于 max heap顶部的值，即小于等于较小的一半里的最大值，则也是 把value放到max heap里即较小的一半里去
else if value的值 大于 max heap顶部的值，即大于较小的一半里的最大值，则 
先把value放到 min heap即较大的一半里，再把较大的一半里的最小值放到较小的一半里去

(2) 如果此时max heap size == min heap size + 1
if value的值 小于等于 max heap顶部的值，即小于等于较小的一半里的最大值，则
先把value放到max heap即较小的一半里，再把较小的一半里的最大值放到较大的一半里去
if value的值大于 max heap顶部的值，即大于较小的一半里的最大值，则把value放到min heap里即较大的一半里去

最后求median的时候，如果2个heap的size相同，则取2个heap的顶部的值再除以2.0；
如果较小的一半比较大的一半多一个数，则取较小的一半的顶部的值，即max heap的顶部的值

### Follow Up
* 如果data stream 里的数特别特别多，怎么办？
只保留median附近的一定数量（足够大的数量，但又不是特别大到放不下）的数，比如：
只保留较小的一半的数里面最大的10000个，以及最大的数里面最小的10000个。
如果来了一个数，比较小的一半的最大的10000个都要小，则扔掉它不管；如果它的值介于这10000个数的范围内，则把它加入这个max heap，
然后把这个max heap的最大的值poll出来，offer到较大的一半的最小的10000个里面去。
如果来了一个数，需要放到较大的一半去，也按类似的方法处理

* 上面两个另外的follow up怎么办？？？？ <=== ？？？？

### Complexity
* Time: O(logn)，logn其实是平均速度，heap不能保证永远是logn
* Space: O(n)

### Java
```java
class MedianFinder {
    private PriorityQueue<Integer> smallerHalf;
    private PriorityQueue<Integer> largerHalf;

    /** initialize your data structure here. */
    public MedianFinder() {
        // min heap
        largerHalf = new PriorityQueue<>();
        // max heap，同时如果一共有奇数个数的话，多出来的一个也放在这里
        smallerHalf = new PriorityQueue<>(10, Collections.reverseOrder());
    }
    
    public void addNum(int value) {
        if (smallerHalf.size() == largerHalf.size()) {
            if (smallerHalf.isEmpty() || value <= smallerHalf.peek()) {
                smallerHalf.offer(value);
            } 
            // value > smallerHalf.peek()
            // 注意，这里的标准并不是value >= biggerHalf.peek()，因为value完全可能落在两个heap之间的区域
            else { 
                largerHalf.offer(value);
                smallerHalf.offer(largerHalf.poll()); // 要多出来一个的话，这一个总是要smallerHalf来扛
            }
        } 
        else { // smallerHalf.size() == largerHalf.size() + 1
            if (value <= smallerHalf.peek()) {
                smallerHalf.offer(value);
                largerHalf.offer(smallerHalf.poll());
            } else { // value > smallerHalf.peek()
                largerHalf.offer(value);
            }
        }
    }
    
    public double findMedian() {
        if (smallerHalf.size() + largerHalf.size() == 0) {
            return Integer.MIN_VALUE;
        } else if (smallerHalf.size() == largerHalf.size()) {
          return (smallerHalf.peek() + largerHalf.peek()) / 2.0;
        } else { // smallerHalf.size() == largerHalf.size() + 1
          return (double)smallerHalf.peek();
        }        
    }
}
```

## Solution 2：用2个TreeSet做
上面用heap的话加一个或者删一个数都未必能做到logn的时间，但是用2个tree set的话就可以确保logn（疑问是，leetcode里实测的速度，这个方法反而比2个heap的方法要慢很多）

TreeSet 是自动平衡的，且自动排序的。TreeSet.first()就是peek最小的那个数，TreeSet.last()就是peek最大的那个数。
所以这种做法也就不用设置一个reverse order的TreeSet。两个TreeSet都用默认排序就行（如果真要设reverse order的TreeSet，语法如下，不需要输入默认size：`treeSet = new TreeSet<Integer>(Collections.reverseOrder())`）
    
注意，stream里可能有重复的数！**重复的数对于2个heap的方法并没有影响，而对于2个tree set的方法影响就很大了**。为了往tree set里放入多个相同的数，我们**把所有的数出现的次序也都记录下来，这样所有的数就都不“重复”了**。具体见下面的代码

### Complexity
* Time: O(logn)，TreeSet 能保证各个操作永远是logn
* Space: O(n)

### Java
```java
class Element {
    int value;
    int index;
    public Element(int v, int i) {
        this.value = v;
        this.index = i;
    }
}

class ElementComparator implements Comparator<Element> {
    @Override
    public int compare(Element e1, Element e2) {
        int result = Integer.compare(e1.value, e2.value);
        if (result == 0) {
            result = Integer.compare(e1.index, e2.index);
        }
        return result;
    } 
}

class MedianFinder {
    private TreeSet<Element> smallerHalf;
    private TreeSet<Element> largerHalf;
    private int count;

    /** initialize your data structure here. */
    public MedianFinder() {
        largerHalf = new TreeSet<>(new ElementComparator());
        // 如果一共有奇数个数的话，多出来的一个也放在这里
        smallerHalf = new TreeSet<>(new ElementComparator());
        count = 0;
    }
    
    public void addNum(int num) {
        Element curEle = new Element(num, this.count);
        this.count++;
        
        if (smallerHalf.size() == largerHalf.size()) {
            if (smallerHalf.isEmpty() || curEle.value <= smallerHalf.last().value) {
                smallerHalf.add(curEle);
            } 
            else { 
                largerHalf.add(curEle);
                smallerHalf.add(largerHalf.pollFirst());
            }
        } 
        else { // smallerHalf.size() == largerHalf.size() + 1
            if (curEle.value <= smallerHalf.last().value) {
                smallerHalf.add(curEle);
                largerHalf.add(smallerHalf.pollLast());
            } else { // value > smallerHalf.peek()
                largerHalf.add(curEle);
            }
        }
    }
    
    public double findMedian() {
        if (smallerHalf.size() + largerHalf.size() == 0) {
            return Integer.MIN_VALUE;
        } else if (smallerHalf.size() == largerHalf.size()) {
          return (smallerHalf.last().value + largerHalf.first().value) / 2.0;
        } else { // smallerHalf.size() == largerHalf.size() + 1
          return (double)smallerHalf.last().value;
        }        
    }
}     
```


## Solution 3: 只用一个tree set，加入数的不同大小情况，来决定median向左还是向右移动一位！用tree set的ceiling或floor来找下一个数！

做一下！！！ <====

## Reference
* [Find Median from Data Stream [LeetCode]](https://leetcode.com/problems/find-median-from-data-stream/description/)

{% include links.html %}
