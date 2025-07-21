# Max Area of Island
[Leetcode Problem](https://leetcode.com/problems/max-area-of-island/description/)

## Problem Description
You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical). You may assume all four edges of the grid are surrounded by water.
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

## Solution Using HashSet (doesn't modify input)

```kotlin
fun maxAreaOfIslandBFS(grid: Array<IntArray>): Int {
    var maxArea = 0
    val visited = HashSet<Pair<Int, Int>>()
    val queue = ArrayDeque<Pair<Int, Int>>()
    
    for(i in grid.indices) {
        for(j in grid[i].indices) {
            if(grid[i][j] == 0 || visited.contains(Pair(i, j))) continue
            
            var area = 0
            queue.add(Pair(i,j))
            while(!queue.isEmpty()) {
                val peek = queue.removeFirst()
                val row = peek.first
                val column = peek.second
                if(
                    row < 0 || 
                    row >= grid.size || 
                    column < 0 ||
                    column >= grid[0].size ||
                    grid[row][column] == 0 ||
                    visited.contains(peek)
                ) continue
                
                visited.add(peek)
                
                area += 1
                queue.add(Pair(row - 1, column))
                queue.add(Pair(row + 1, column))
                queue.add(Pair(row, column - 1))
                queue.add(Pair(row, column + 1))
            }
            
            maxArea = maxOf(maxArea, area)
        }
    }
    
    return maxArea
}

```


## Solution Modifying grid in-place (more memory efficient) - Changing 1 for 0

```kotlin
fun maxAreaOfIslandBFSInPlace(grid: Array<IntArray>): Int {
    var maxArea = 0
    val queue = ArrayDeque<Pair<Int, Int>>()
    
    for(i in grid.indices) {
        for(j in grid[i].indices) {
            if(grid[i][j] == 0) continue
            
            var area = 0
            queue.add(Pair(i, j))
            grid[i][j] = 0 // Mark as visited immediately
            
            while(queue.isNotEmpty()) {
                val (row, col) = queue.removeFirst()
                area++
                
                val neighbors = listOf(
                    Pair(row - 1, col),
                    Pair(row + 1, col),
                    Pair(row, col - 1),
                    Pair(row, col + 1)
                )
                
                for((nr, nc) in neighbors) {
                    if(nr in 0 until grid.size && 
                       nc in 0 until grid[0].size && 
                       grid[nr][nc] == 1) {
                        grid[nr][nc] = 0 // Mark as visited
                        queue.add(Pair(nr, nc))
                    }
                }
            }
            
            maxArea = maxOf(maxArea, area)
        }
    }
    
    return maxArea
}
```
