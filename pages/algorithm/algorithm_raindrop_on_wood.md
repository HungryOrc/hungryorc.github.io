---
title: "Raindrop on Wood"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_raindrop_on_wood.html
folder: algorithm
toc: false
---

## Description
有一块木头长100cm。设木头的方向为x轴，木头的一端为0，另一端为100。雨滴可以打到任何的x坐标，包括小于0或大于100的坐标。雨滴的个数可以非常大。每个雨滴的宽度都是1cm。每个雨滴打湿的范围都是 `[start, start + 1)`，注意左边是闭区间，右边是开区间，`start`是一个double数值。实现3个API：
* void raindrop(double start); // drop a raindrop
* boolean isWet(double spot); // spot is an x axis cooridate in range [0, 100]
* boolean isFullyWet(); // the whole wood is wet or not

## Example

## Solution
把这个100cm的木头分成100段，每段都是 `[i, i+1)`，其中i是0到99的整数。当然还有一个点要算上，就是坐标100的那一个点。

关键点：
* 上述分段得到的每一段木头，无论多少雨滴怎么打它，它上面的dry part都一定只有一段，不会有两段！
  * 比如对于 `[i, i+1)`，如果雨点的起始点正好在i坐标，那么这一整段都正好湿了
  * 如果雨点的起始点在i的左边，设为j，那么这一段上的dry part会变为 `[j+1, i+1)`，如果又滴下来一个雨点，它的起始点在j和i+1之间，设为k，则dry part会变为 `[j+1, k)`。继续下去，可能会有很多雨点，把这一段都打湿
  * 如果雨点的起始点正好在 i+1 处，那么对于这个小段的木头，没有影响
* 任何一个雨滴，它最多能打湿两个小段的木头（的各一部分）

所以可以这么做：
* 搞一个 class Section，有个 int type 起始点（那么终点自然也有知道了），用来表示 `[i, i+1)` 这一段木头 
* 整个一米长的木头就是 array of Sections
* 每个 Section 维持一个 dry part，dry part要有起始点和终止点，dry part的形状是前闭区间后开区间。每次有一滴雨滴下来，我们就要update一个或两个Section的dry part数据
* 用一个hashset来维护所有的没全湿的Sections。如果一个Section全湿了，即它的dry part不存在了，那么就把这个Section从这个hashset里去掉
* 要查某一点当前是否wet，就先找到那个Section（用double向下取整就行），然后看这个Section是否全湿了，如果它没全湿，就看它的dry part是否包含这一点
* 要查整个木头是否都全湿了，就看之前所说的那个hashset是否空了。还要特别看一下100那个点是否湿了。

### Complexity
* Time
  * void raindrop(double start); // O(1)
  * boolean isWet(double spot); // O(1)
  * boolean isFullyWet(); // O(1)
* Space: O(100)，如果认为木头的长度是m厘米，则空间复杂度为 O(m)。这个解法用到了好几个数据结构，但它们的空间都是 O(m)

### Java
```java
class Section {
    public double start;
    public double dryStart, dryEnd;
    
    public Section(double start) {
    	this.start = start;
    	this.dryStart = start;
    	this.dryEnd = start + 1;
    }
}

public class Solution {
	public boolean wetAt100;
	public Section[] wood;
	public HashSet<Integer> nonWetSections;
	
	public Solution() {
		wetAt100 = false;
		
	    wood = new Section[100];
	    for (int i = 0; i < 100; i++) {
	    	wood[i] = new Section(i);
	    }
	    
	    nonWetSections = new HashSet<>();
	    for (int i = 0; i < 100; i++) {
	        nonWetSections.add(i);
	    }
	}
    
    public void raindrop(double start) {
        if (start < 0 && start > -1) {
            updateDryZone(0, start);
        } else if (start > 99 && start <= 100) {
            updateDryZone(99, start);
            wetAt100 = true;
        } else if (start >= 0 && start <= 99) {
            int sectionIndex = (int)Math.floor(start);
            updateDryZone(sectionIndex, start);
            
            // the current raindrop may affect the next Section
            if (start > sectionIndex) {
                updateDryZone(sectionIndex + 1, start);
            }
        }
    }
    
    private void updateDryZone(int sectionIndex, double start) {
        // if the current section is already fully wet, then we do nothing
        if (!nonWetSections.contains(sectionIndex)) {
            return;
        }
        
        Section section = wood[sectionIndex];
        if (start < sectionIndex) {
            section.dryStart = Math.max(start + 1, section.dryStart);
        } else { // start >= sectionIndex
            section.dryEnd = Math.min(start, section.dryEnd);
        }
        
        // check if current section is now all wet
        if (section.dryEnd <= section.dryStart) {
            nonWetSections.remove(sectionIndex);
        }
    }
    
    // spot is guaranteed to be in the range [0, 100]
    public boolean isWet(double spot) {
        if (spot == 100) {
            return wetAt100;
        }
        
        int sectionIndex = (int)Math.floor(spot);
        Section section = wood[sectionIndex];
        if (section.dryStart <= spot && section.dryEnd > spot) {
            return false;
        } else {
            return true;
        }
    }
    
    public boolean isFullyWet() {
        return nonWetSections.isEmpty();
    }
    
    // -------------------------------------------------------------
    // main
    
    public static void main(String[] args) {
        Solution solu = new Solution();
        
        solu.raindrop(-0.3);
        System.out.println(solu.isWet(0.5)); // true
        
        solu.raindrop(99);
        System.out.println(solu.isWet(100)); // false
        solu.raindrop(99.0001);
        System.out.println(solu.isWet(100)); // true
        
        System.out.println(solu.isFullyWet()); // false
    }
}
```

## Reference


{% include links.html %}
