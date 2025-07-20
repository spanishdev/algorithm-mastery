# Number of Islands
[Leetcode Problem](https://leetcode.com/problems/number-of-islands/description/)

## Problem Description
Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.
An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.


## Examples

```kotlin
Example 1:
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1

Example 2:
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3

```

## Constraints
- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 300
- grid[i][j] is '0' or '1'.

## Key Insights
- Each connected component of '1's forms one island
- Use DFS or BFS to mark all connected land cells when finding an island
- Once a cell is visited, mark it to avoid counting it again
- Count how many times we start a new DFS/BFS traversal

## Solution Approach
- Iterate through each cell in the grid
- When we find an unvisited '1', increment island counter
- Use DFS/BFS to mark all connected '1's as visited
- Continue until all cells have been processed
- Return the total count of islands found

## Complexity Analysis
- Time Complexity: O(m × n) where m and n are grid dimensions
- Space Complexity: O(m × n) in worst case for recursion stack (DFS) or queue (BFS)

## Solution 1: Using HashSet as visited

```kotlin
fun numIslands(grid: Array<CharArray>): Int {
    val visited: HashSet<Cell> = mutableSetOf()
    var islandCount = 0
    
    for(row in 0 until grid.size) {
        for(column in 0 until grid[row].size) {
            val cell = Cell(row, column)
            
            if(cell.getValue(grid) == '1' && !visited.contains(cell)) {
                exploreIsland(cell, grid, visited)
                islandCount++
            }
        }
    }
    
    return islandCount
}

private fun exploreIsland(
    cell: Cell, 
    grid: Array<CharArray>,
    visited: HashSet<Cell>
) = with(cell) {
    if(
        row < 0 || 
        row >= grid.size || 
        column < 0 || 
        column >= grid[row].size ||
        getValue(grid) == '0' ||
        visited.contains(this)
    ) return
        
    
    visited.add(this)
    
    exploreIsland(this.copy(row + 1, column), grid, visited)
    exploreIsland(this.copy(row - 1, column), grid, visited)
    exploreIsland(this.copy(row, column + 1), grid, visited)
    exploreIsland(this.copy(row, column - 1), grid, visited)
}

data class Cell(val row: Int, val column: Int) {
    fun getValue(grid: Array<CharArray>) = grid[row][column]
}
```


## Solution 2: Writing on Grid and marking visited as '0'

```kotlin
fun numIslands(grid: Array<CharArray>): Int {
    var islands = 0
    
    fun dfs(row: Int, col: Int) {
        if (row < 0 || row >= grid.size || col < 0 || col >= grid[0].size || grid[row][col] != '1') return
        
        grid[row][col] = '0' // Mark as visited
        dfs(row + 1, col)
        dfs(row - 1, col)
        dfs(row, col + 1)
        dfs(row, col - 1)
    }
    
    for (row in grid.indices) {
        for (col in grid[0].indices) {
            if (grid[row][col] == '1') {
                dfs(row, col)
                islands++
            }
        }
    }
    
    return islands
}

```
