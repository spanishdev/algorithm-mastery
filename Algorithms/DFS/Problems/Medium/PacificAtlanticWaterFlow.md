# Pacific Atlantic Water Flow
[Leetcode Problem](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)

## Problem Description
There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.
The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where heights[r][c] represents the height above sea level of the cell at coordinate (r, c).
The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into that ocean.
Return a 2D list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

## Examples

<img width="573" height="573" alt="imagen" src="https://github.com/user-attachments/assets/d0489d98-77dd-420f-b8fb-6bd870fe34e2" />

```kotlin

Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.

Example 2:

Input: heights = [[1]]
Output: [[0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.

```

## Constraints
- m == heights.length
- n == heights[r].length
- 1 <= m, n <= 200
- 0 <= heights[r][c] <= 105

## Key Insights
- Reverse thinking: Instead of checking if each cell can reach both oceans, start from ocean edges and see which cells they can reach
- Water flows from higher to lower or equal height
- When going backwards (from ocean), we go from lower to higher or equal height
- A cell can flow to both oceans if it's reachable from both Pacific and Atlantic edges
- Use DFS/BFS from all Pacific border cells and all Atlantic border cells separately

## Solution Approach
- Create two boolean matrices to track cells reachable from Pacific and Atlantic
- Start DFS/BFS from all Pacific border cells (top row + left column)
- Start DFS/BFS from all Atlantic border cells (bottom row + right column)
- For each ocean, mark all cells reachable through valid water flow
- Find intersection: cells reachable from both oceans
- Return coordinates of intersection cells

## Complexity Analysis
- Time Complexity: O(m × n) where m and n are grid dimensions
- Space Complexity: O(m × n) for the two reachable matrices

## Solution

```kotlin
fun pacificAtlantic(heights: Array<IntArray>): List<List<Int>> {
    val atlanticVisited = HashSet<Pair<Int, Int>>()
    val pacificVisited = HashSet<Pair<Int, Int>>()
    
    for(r in heights.indices) {
        for(c in heights[0].indices) {
            if(r == 0 || c == 0) checkOcean(heights, r, c, pacificVisited, 0)
            if(r == heights.size - 1 || c == heights[0].size - 1) checkOcean(heights, r, c, atlanticVisited, 0)
        }
    }
    
    val result = MutableList<List<Int>>()
    for(r in heights.indices) {
        for(c in heights[0].indices) {
            val pair = Pair(r,c)
            
            if(atlanticVisited.contains(pair) && pacificVisited.contains(pair)) {
                result.add(listOf(pair.first,pair.second))
            }
        }
    }
    
    return result.toList()
}

private fun checkOcean(
    heights: Array<IntArray>, 
    row: Int,
    column: Int, 
    visited: HashSet<Pair<Int, Int>>,
    previousValue: Int,
) {
    if(row < 0 || row >= heights.size || column < 0 || column >= heights[0].size) return
    
    val pair = Pair(row, column)
    val value = heights[row][column]
    if(visited.contains(pair) || value < previousValue) return
    
    visited.add(pair)
    checkOcean(heights, row + 1, column, visited, value)
    checkOcean(heights, row - 1, column, visited, value)
    checkOcean(heights, row, column + 1, visited, value)
    checkOcean(heights, row, column - 1, visited, value)
}

```
