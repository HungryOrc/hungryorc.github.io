---
title: "Hash Map in C++: \"unordered_map\""
tags: [c++]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: c++_hashmap.html
folder: c++
toc: false
---

在 C++ 里，`map` 的内部实现是 红黑树，而非 Hash Map。内部实现为 Hash Map 的是 `unordered_map`。

```c++
unordered_map<int, int> startIndex, count; // 2 Hash Maps

// ...

for (int i = 0; i < nums.size(); i++) {
    int ni = nums[i];
    if (startIndex.find(ni) == startIndex.end()) { // == unordered_map.end() 表示不存在，即iterate到尾部还是没能找到
        startIndex[ni] = i; // 这样可以直接写hashmap里的一个entry
    }

    int curLen = i - startIndex[ni] + 1;
    int curCount = ++count[ni]; // 这样可以直接读hashmap里的一个entry，不管他存不存在，不存在的话 int 变量默认为 0

    // ...
}
```


## Reference

* []()

{% include links.html %}