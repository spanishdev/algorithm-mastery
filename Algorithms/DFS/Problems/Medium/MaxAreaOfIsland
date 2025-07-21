# Max Area of Island
[Leetcode Problem](https://leetcode.com/problems/max-area-of-island/description/)

## Problem Description
You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical). 
You may assume all four edges of the grid are surrounded by water.
The area of an island is the number of cells with a value 1 in the island.
Return the maximum area of an island in grid. If there is no island, return 0.

## Examples

```kotlin
Example 1:
Input: grid = [
  [0,0,1,0,0,0,0,1,0,0,0,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,1,1,0,1,0,0,0,0,0,0,0,0],
  [0,1,0,0,1,1,0,0,1,0,1,0,0],
  [0,1,0,0,1,1,0,0,1,1,1,0,0],
  [0,0,0,0,0,0,0,0,0,0,1,0,0],
  [0,0,0,0,0,0,0,1,1,1,0,0,0],
  [0,0,0,0,0,0,0,1,1,0,0,0,0]
]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.

Example 2:
Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0

```

## Constraints
- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 50
- grid[i][j] is either 0 or 1.

## Key Insights
- Similar to Number of Islands but track area instead of count
- Use DFS or BFS to explore each island and count cells
- Return the maximum area encountered across all islands
- Each connected component represents one island

## Solution Approach
- Iterate through each cell in the grid
- When we find an unvisited '1', start DFS/BFS to calculate island area
- During traversal, count each '1' cell visited
- Track the maximum area found so far
- Return the maximum area after checking all islands

## Complexity Analysis
- Time Complexity: O(m × n) where m and n are grid dimensions
- Space Complexity: O(m × n) in worst case for recursion stack (DFS) or queue (BFS)

## Solution using Hashset

```kotlin
fun maxAreaOfIsland(grid: Array<IntArray>): Int {
    var maxArea = 0
    val visited = HashSet<Pair<Int, Int>>()
    
    for(i in grid.indices) {
        for(j in grid[i].indices) {
            val value = grid[i][j]
            if(value == 0 || visited.contains(Pair(i, j))) continue
            
            val area = calculateArea(grid, i, j, visited)
            maxArea = maxOf(maxArea, area)
        }
    }
    
    return maxArea
}

private fun calculateArea(grid: Array<IntArray>, row: Int, column: Int, visited: HashSet<Pair<Int, Int>>): Int {
    if(
        row < 0 || 
        row >= grid.size ||
        column < 0 ||
        column >= grid[0].size || 
        grid[row][column] == 0 ||
        visited.contains(Pair(row, column))
    ) return 0
    
    visited.add(Pair(row, column))
    
    return 1 + calculateArea(grid, row - 1, column, visited) +
               calculateArea(grid, row + 1, column, visited) +
               calculateArea(grid, row, column - 1, visited) +
               calculateArea(grid, row, column + 1, visited)
}
```


## Solution Modifying grid in-place (more memory efficient) - Changing 1 for 0

```kotlin
fun maxAreaOfIslandInPlace(grid: Array<IntArray>): Int {
    var maxArea = 0
    
    fun dfs(row: Int, col: Int): Int {
        if (row < 0 || row >= grid.size || col < 0 || col >= grid[0].size || grid[row][col] != 1) {
            return 0
        }
        
        grid[row][col] = 0 // Mark as visited by changing to water
        
        // Count current cell + all connected cells
        return 1 + dfs(row + 1, col) + dfs(row - 1, col) + dfs(row, col + 1) + dfs(row, col - 1)
    }
    
    for (row in grid.indices) {
        for (col in grid[0].indices) {
            if (grid[row][col] == 1) {
                maxArea = maxOf(maxArea, dfs(row, col))
            }
        }
    }
    
    return maxArea
}

```
