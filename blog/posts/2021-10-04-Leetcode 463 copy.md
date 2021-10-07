---
layout: post
title: "Leetcode 463"
subtitle: "Island Perimeter"
author: "Sailorlqh"
date: 2021-10-04
header_img: /img/home-bg/27.jpg
catalog: true
tags:
  - Leetcode

---

# 463. Island Perimeter

Question link is [here](https://leetcode.com/problems/island-perimeter/).

### Question

You are given `row x col` `grid` representing a map where `grid[i][j] = 1` represents land and `grid[i][j] = 0` represents water.

Grid cells are connected **horizontally/vertically** (not diagonally). The `grid` is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/10/12/island.png)

```
Input: grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
Output: 16
Explanation: The perimeter is the 16 yellow stripes in the image above.
```

**Example 2:**

```
Input: grid = [[1]]
Output: 4
```

**Example 3:**

```
Input: grid = [[1,0]]
Output: 4
```

### Solution

Easy level question.

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int sum = 0;
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[0].length; j++) {
                if(grid[i][j] == 1) {
                    sum += 4;
                    if(i-1 >= 0 && grid[i-1][j] == 1) {
                        sum -= 2;
                    }
                    if( j-1 >= 0 && grid[i][j-1] == 1) {
                        sum -= 2;
                    }
                }
            }
        }
        return sum;
    }
}
```

Time Complexity: $O(mn)$, where m, n are the height and width of the grid. 

