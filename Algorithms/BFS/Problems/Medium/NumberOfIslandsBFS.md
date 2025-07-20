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

## Solution 1: Using Queue and HashSet as visited

```kotlin
fun numIslandsBFS(grid: Array<CharArray>): Int {
    var islandCount = 0
    val queue = ArrayDeque<Cell>()
    val visited = HashSet<Cell>()
    
    for(i in grid.indices) {
        for(j in grid[i].indices) {
            val cell = Cell(i, j)
            if(cell.getValue(grid) == '0' || visited.contains(cell)) continue
            
            queue.add(cell)
            
            while(!queue.isEmpty()) {
                with(queue.removeFirst()) {
                    if(getValue(grid) == '0' || visited.contains(this)) continue
                    
                    visited.add(this)
                    if(column > 0) queue.add(this.copy(row, column - 1))
                    if(column < grid[row].size - 1) queue.add(this.copy(row, column + 1))
                    if(row > 0) queue.add(this.copy(row - 1, column))
                    if (row < grid.size - 1) queue.add(this.copy(row + 1, column))
                }
            }
            
            islandCount++
        }
    }
    
    return islandCount
}

data class Cell(val row: Int, val column: Int) {
    fun getValue(grid: Array<CharArray>) = grid[row][column]
}
```


## Solution 2: Writing on Grid and marking visited as '0'

```kotlin
// BFS Solution - Modifying grid in-place
fun numIslandsBFS(grid: Array<CharArray>): Int {
    var islandCount = 0
    val queue = ArrayDeque<Cell>()
    
    for(i in grid.indices) {
        for(j in grid[i].indices) {
            val cell = Cell(i, j)
            if(cell.getValue(grid) == '0') continue
            
            // Marcar como visitado ANTES de añadir a queue
            grid[i][j] = '0'
            queue.add(cell)
            
            while(!queue.isEmpty()) {
                with(queue.removeFirst()) {
                    // Explorar vecinos
                    val neighbors = listOf(
                        Cell(row, column - 1),
                        Cell(row, column + 1), 
                        Cell(row - 1, column),
                        Cell(row + 1, column)
                    )
                    
                    neighbors.forEach { neighbor ->
                        if (neighbor.row in 0 until grid.size && 
                            neighbor.column in 0 until grid[0].size &&
                            neighbor.getValue(grid) == '1') {
                            
                            grid[neighbor.row][neighbor.column] = '0' // Marcar como visitado
                            queue.add(neighbor)
                        }
                    }
                }
            }
            
            islandCount++
        }
    }
    
    return islandCount
}

```
