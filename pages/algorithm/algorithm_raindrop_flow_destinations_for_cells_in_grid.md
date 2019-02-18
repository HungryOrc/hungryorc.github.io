---
title: "Raindrop Flow Destinations for Cells in a 2D Grid"
tags: [algorithm]
keywords:
summary:
sidebar: mydoc_sidebar
permalink: algorithm_raindrop_flow_destinations_for_cells_in_grid.html
folder: algorithm
toc: false
---

## Description
2D grid，里面每个元素是一个非负整数，表示当前cell的height。grid四周以外认为是高度无限高的石壁。开始下雨。
* 每个雨滴掉到一个cell的时候，这个雨滴可以往这个cell上下左右的四个方向的邻居cell流。它会向高度最低的那个邻居cell流。
然后再向那个邻居cell的高度最低的邻居cell流，以此类推。
* 如果有2个或更多的邻居cell的高度并列最低，则随意流向任何一个邻居cell都行
* 如果最低的邻居cell和当前cell的高度一样，则雨滴留在当前cell
* 如果当前cell比所有邻居cell的高度都低，自然雨滴也是要留在当前cell的

求对于所有的cells，滴到它们上面的雨滴，最终都分别流到哪些cell上去了。这些一一对应的关系，最后用一个 Map<List<Integer>, List<Integer>> 来表示。
作为key的list里面有两个int，分别是雨滴最初滴到的那个cell的x和y。
作为value的list里面也有两个int，分别是雨滴最终流到的那个cell的x和y。

### Example
见下面的代码里的main函数

## Solution：我自己的方法，DFS
哦也

### Complexity
* Time: O(n^3) <=== 对么 ？？？
* Space: O(n^2) <=== 对么 ？？？

### Java
代码虽然看起来不短，其实逻辑不复杂
```java
public class Solution {
    private static final int[][] DIRS = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    
    public Map<List<Integer>, List<Integer>> findDestinations(int[][] grid) {
        // ignore sanity checks
        
        Map<List<Integer>, List<Integer>> result = new HashMap<>();
        int n = grid.length, m = grid[0].length;
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                findDest(grid, i, j, result);
            }
        }
        return result;
    }
    
    private List<Integer> findDest(
            int[][] grid, int i, int j, Map<List<Integer>, List<Integer>> result) {
        List<Integer> curCell = new ArrayList<>();
        curCell.add(i);
        curCell.add(j);
        
        List<Integer> destCell = result.get(curCell);
        
        if (destCell != null) {
            return destCell; // 向上传递到父，祖父，曾祖父...
        }
        
        int curHeight = grid[i][j];
        int lowestHeight = curHeight;
        destCell = curCell;
        
        for (int[] dir : DIRS) {
            int x = i + dir[0], y = j + dir[1];
            
            if (!isInbound(grid.length, grid[0].length, x, y)) {
                continue;
            }
            
            if (grid[x][y] < lowestHeight) {
                lowestHeight = grid[x][y];
                destCell = new ArrayList<Integer>();
                destCell.add(x);
                destCell.add(y);
            }
        }
        
        // 如果当前cell的高度 <= 周围四个cells的高度，那么当前cell就是自己的destination cell
        if (destCell == curCell) {
            result.put(curCell, curCell);
        } else {
            System.out.println(destCell.get(0) + ", " + destCell.get(1));
            destCell = findDest(grid, destCell.get(0), destCell.get(1), result);
            result.put(curCell,  destCell);
        }
        return destCell;
    }
    
    private boolean isInbound(int n, int m, int i, int j) {
        return (i >= 0 && j >= 0 && i < n && j < m);
    }
    
    // ------------------------------------------------------
    // main
    public static void main(String[] args) {
        int[][] grid = {{1, 2, 3}, {2, 5, 6}, {0, 8, 9}};
        
        Solution solu = new Solution();
        Map<List<Integer>, List<Integer>> result = solu.findDestinations(grid);
        System.out.println(result);
        /* The result (map) should be: 
         * 注：等号前面的是map里的key，后面的是value
         * {
         *     [1, 0]=[2, 0], [2, 1]=[2, 0], [0, 0]=[0, 0], 
         *     [1, 1]=[2, 0], [2, 2]=[0, 0], [0, 1]=[0, 0], 
         *     [1, 2]=[0, 0], [0, 2]=[0, 0], [2, 0]=[2, 0]
         * }
         */
    }
}
```

## Reference
网上没找到这题

{% include links.html %}
