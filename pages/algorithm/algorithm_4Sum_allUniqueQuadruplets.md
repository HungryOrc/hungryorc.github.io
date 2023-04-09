

## Solution：

本题需要给出所有可能的组合，且不能重复，这个要求比仅仅判断true/false高很多。
给的数组里的数字可能有重复的，而**数组排序以后，很可能有利于去重**，所以我们先把数组排序。

我们需要把 HashMap 从简单的记录 <sum, pair> 改进到记录 <sum, 所有的和为sum的pairs>，这样才能最终得到所有的组合。
注意每个pair记录的是一对elements' indexes，而不是一对elements' values。

**Java 可以把 List<Integer> 放到set或map里做key，Java的Set/Map对List的equals和hashcode这两个函数都有正确的默认的处理。不能把 int[] 放到set/map里做key，Java的Set/Map对int[]的 equals and/or hashcode 的默认处理是不正确的，且无法override**

我们用一个长度为2的List来表示一对 elements' indexes，list里的第一个元素就是left element's index，第二个元素就是right element's index。所以总的来说就是：
```
// <pair中2个数的和，和等于前面的key的所有pairs的indexes>
HashMap<Integer, HashSet<List<Integer>>> map;
```

然后，因为数组里的数字有重复，所以我们光保证 "左pair的右index < 右pair的左index" 是不够的。
比如如果target是20，数组里有4个5（排序以后，这4个5会贴在一起），我们找到第一个pair of 5以后，还会找到第二个pair of 5。注意前一个pair的right index 小于 后一个pair的 left index。这两个pair的和是target，应该入选result。这些都没问题。问题在于，如果数组里有6个5，那么就会有第三对5。这时候就需要在result里去重。我目前的做法是，对于每个要放到result里去的quadruplet，都先放到一个HashSet<List<Integer>>里去去重一下，再放到result里去。

要注意的一些细节：
* 因为我们在pair里放的是indexes，不是element values，所以用下面的双层for loop 搞出来的任何pairs都永远不会重复。
* 因为我们首先要把数组排序，所以任何一个pair所代表的element values 都不会出现后一个比前一个小的情况，只能是后一个大于等于前一个。然后任何的前一个pair的第二个element value 都会小于等于后一个pair的第一个element value，即不会出现依次找到两个pair [4, 5] 和 [4, 5] 的情况（注意这里说的不是indexes）
* 对于遇到的每一个pair，如果它里面的2个indexes所代表的2个elements的values的sum（下文直接简称为 sum）在以前出现过，我们就要分情况讨论：
  * 如果这个sum不等于target的一半，那么把这个pair 放到map 里就完事了
  * 如果这个sum等于target的一半，那么对于map里所有的sum等于这个sum的pairs，只要那些pair的right index 小于当前pair的left index，那么这两个pair都可以组成一个答案，但最后还是要去重，因为上面说的，可能会出现3个[5,5],[5,5],[5,5]的情况（target为20时）。
* 如果对于一个pair的sum，之前存在（一个或几个）pairs 的 sum 与它加在一起等于target（注意到了这里都是 sum 不等于 target 的一半的情况）：
  * 那么那些 pairs 都可能可以和 current pair 组合成一个quadruplet，不过还是要经过上文说的两关：第一，前一个pair的right index < 后一个pair的left index；第二，整个quadruplet要去重




