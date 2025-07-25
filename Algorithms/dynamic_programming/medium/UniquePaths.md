# Unique Paths
[Leetcode Problem](https://leetcode.com/problems/unique-paths/description/)

## Problem Description
There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). 
The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.
Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.
The test cases are generated so that the answer will be less than or equal to 2 * 109.

## Examples

### Example 1

<img width="400" height="183" alt="robot_maze" src="https://github.com/user-attachments/assets/8a734a79-5a33-4c88-ab02-baaa5d37b909" />

```kotlin
Input: m = 3, n = 7
Output: 28
```


### Example 2

```kotlin
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

## Constraints
- 1 <= m, n <= 100

## Key Insights
### Core Insight:
To reach any cell (i,j), you can only come from:
- Cell (i-1, j) - from above (move down)
- Cell (i, j-1) - from left (move right)

### DP State Definition:
dp[i][j] = number of unique paths to reach cell (i,j)

### Recurrence Relation:
dp[i][j] = dp[i-1][j] + dp[i][j-1]

### Key Observations:
- First row: Only 1 way to reach (can only move right)
- First column: Only 1 way to reach (can only move down)
- Any other cell: Sum of paths from above and left

### Mathematical insight:
This is a combinatorics problem. To reach (m-1,n-1), you need:
- (m-1) down moves
- (n-1) right moves
- Total moves = (m-1) + (n-1) = m + n - 2
- Answer = C(m+n-2, m-1) = (m+n-2)! / ((m-1)! × (n-1)!)

## Solution Approach

### Approach 1: 2D DP (Most Intuitive)
Create 2D array dp[m][n]
Fill first row and column with 1s
Fill rest using recurrence relation

### Approach 2: Space Optimized 1D DP
Only need previous row to compute current row
Use 1D array, update in-place

### Approach 3: Mathematical Formula
Direct combinatorics calculation
O(min(m,n)) time, O(1) space

### Approach 4: Recursive with Memoization
Top-down DP approach
Good for understanding the problem structure

## Complexity Analysis
### 2D DP:
Time: O(m × n) - fill entire grid
Space: O(m × n) - 2D array

### 1D DP (Space Optimized):
Time: O(m × n) - same computation
Space: O(n) - only one row

### Mathematical Formula:
Time: O(min(m,n)) - for computing combinations
Space: O(1) - constant space

### Recursive + Memo:
Time: O(m × n) - each cell computed once
Space: O(m × n) - memo table + recursion stack

## Solutions

### Approach 1-1: 2D DP (Most Intuitive)

```kotlin
    fun uniquePaths(m: Int, n: Int): Int {
        val dp = Array(m) { IntArray(n) }

        dp[0][0] = 1

        for(row in 0 until m) {
            for(column in 0 until n){
                if(row == 0 && column == 0) continue

                var sum = 0
                if(row > 0) sum +=  dp[row - 1][column]
                if(column > 0) sum +=  dp[row][column - 1]
                   
                dp[row][column] = sum
            }
        }

        return dp[m - 1][n - 1]
    }
```
### Approach 1-2: 2D DP (Most Intuitive)

```kotlin
    fun uniquePaths(m: Int, n: Int): Int {
        // Create 2D DP array
        val dp = Array(m) { IntArray(n) }
        
        // Initialize first row (can only move right)
        for (j in 0 until n) {
            dp[0][j] = 1
        }
        
        // Initialize first column (can only move down)
        for (i in 0 until m) {
            dp[i][0] = 1
        }
        
        // Fill the rest of the grid
        for (i in 1 until m) {
            for (j in 1 until n) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
            }
        }
        
        return dp[m-1][n-1]
    }

```

### Approach 2: Space Optimized 1D DP

```kotlin
    fun uniquePathsOptimized(m: Int, n: Int): Int {
        // Only need previous row to compute current row
        var dp = IntArray(n) { 1 }  // Initialize with 1s (first row)
        
        // For each row starting from second
        for (i in 1 until m) {
            for (j in 1 until n) {
                dp[j] = dp[j] + dp[j-1]  // dp[j] = above + left
            }
        }
        
        return dp[n-1]
    }
```

### Approach 3: Mathematical Formula (Most Efficient)

```kotlin
    fun uniquePathsMath(m: Int, n: Int): Int {
        // We need (m-1) down moves and (n-1) right moves
        // Total moves = m + n - 2
        // Answer = C(m+n-2, m-1) = C(m+n-2, n-1)
        
        val totalMoves = m + n - 2
        val downMoves = m - 1
        
        // Calculate C(totalMoves, downMoves)
        var result = 1L
        for (i in 1..minOf(downMoves, totalMoves - downMoves)) {
            result = result * (totalMoves - i + 1) / i
        }
        
        return result.toInt()
    }
```
### Approach 4: Recursive with Memoization

```kotlin
    fun uniquePathsRecursive(m: Int, n: Int): Int {
        val memo = Array(m) { IntArray(n) { -1 } }
        return uniquePathsHelper(m - 1, n - 1, memo)
    }
    
    private fun uniquePathsHelper(i: Int, j: Int, memo: Array<IntArray>): Int {
        // Base cases
        if (i == 0 || j == 0) return 1
        
        // Check memo
        if (memo[i][j] != -1) return memo[i][j]
        
        // Compute and memoize
        memo[i][j] = uniquePathsHelper(i-1, j, memo) + uniquePathsHelper(i, j-1, memo)
        return memo[i][j]
    }
```
