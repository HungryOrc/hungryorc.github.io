---
title: "Color Fill in Matrix"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_color_fill_in_matrix.html
folder: algorithm
toc: false
---

## Description
An image is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).

Given a coordinate (sr, sc) representing the starting pixel (row and column) of the color fill, and a pixel value newColor, "color fill" the image.

To perform a "color fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.
At the end, return the modified image.


### Example
略

## Solution
BFS

### Complexity
* Time: O(n * m)
* Space: O(n * m), each layer of call stack uses O(1) space

### Java
```java
class BFS {
    public int[][] colorFill(int[][] image, int sr, int sc, int newColor) {
        if (image == null || image.length == 0 || image[0].length == 0) {
            return null;
        }
        if (sr < 0 || sc < 0 || sr >= image.length || sc >= image[0].length) {
            return image;
        }
        if (image[sr][sc] == newColor) {
            return image;
        }
    
        fill(image, sr, sc, newColor, image[sr][sc]);
        return image;
    }
    
    // private，static，final：装 X 三连！
    private static final int[][] DIRS = {
        {1,0}, {-1,0}, {0,1}, {0,-1}
    };
    
    private void fill(int[][] image, int sr, int sc, int newColor, int oldColor) {
        if (sr < 0 || sc < 0 || sr >= image.length || sc >= image[0].length) {
            return;
        }
        if (image[sr][sc] != oldColor) {
            return;
        }
        
        image[sr][sc] = newColor;
        
        for (int[] dir : DIRS) {
            fill(image, sr + dir[0], sc + dir[1], newColor, oldColor);
            // or: i = 0 ~ 3, and then:
            // fill(image, sr + DIRS[i][0], sc + DIRS[i][1], newColor, oldColor);
        }
    }
}
```

## Reference
* 本题没有找到网上对应题目

{% include links.html %}
