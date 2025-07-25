# Climbing Stairs
[Leetcode Problem](https://leetcode.com/problems/climbing-stairs/description/)

## Problem Description
You are climbing a staircase. It takes n steps to reach the top.
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

## Examples

```kotlin
Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

```

## Constraints
- 1 <= n <= 45

## Key Insights

### Pattern Recognition:
This is the classic Fibonacci sequence in disguise!

### Core Insight:
To reach step n, you can come from:
- Step n-1 (take 1 step)
- Step n-2 (take 2 steps)

Therefore: ways(n) = ways(n-1) + ways(n-2)

### DP Characteristics:
Optimal Substructure: Solution depends on optimal solutions of subproblems
Overlapping Subproblems: Same subproblems calculated multiple times
Recurrence Relation: dp[i] = dp[i-1] + dp[i-2]

### Multiple Approaches:
Memoization (Top-Down DP) - Recursive with caching
Tabulation (Bottom-Up DP) - Iterative with array
Space Optimized - Only store last 2 values
Mathematical - Direct Fibonacci formula

## Solution Approach

### Approach 1: Space Optimized Bottom-Up (Best)
Only store previous 2 values
O(n) time, O(1) space

### Approach 2: Bottom-Up Tabulation
Fill DP array from bottom to top
O(n) time, O(n) space

### Approach 3: Top-Down Memoization
Recursive with memoization
O(n) time, O(n) space

## Complexity Analysis

### Space Optimized:
Time: O(n) - single pass
Space: O(1) - only 2 variables

### Tabulation:
Time: O(n) - fill array once
Space: O(n) - DP array

### Memoization:
Time: O(n) - each subproblem solved once
Space: O(n) - recursion stack + memo


## Solution


## Approach 1: Space Optimized (Best for interviews)

```kotlin
  fun climbStairs(n: Int): Int {
        if (n <= 2) return n
        
        var prev2 = 1  // ways(0) = 1
        var prev1 = 2  // ways(1) = 1, ways(2) = 2
        
        for (i in 3..n) {
            val current = prev1 + prev2
            prev2 = prev1
            prev1 = current
        }
        
        return prev1
    }

```


## Approach 2: Space Optimized (Best for interviews)

```kotlin
  fun climbStairsTabulation(n: Int): Int {
        if (n <= 2) return n
        
        val dp = IntArray(n + 1)
        dp[1] = 1
        dp[2] = 2
        
        for (i in 3..n) {
            dp[i] = dp[i - 1] + dp[i - 2]
        }
        
        return dp[n]
    }
```


## Approach 3: Top-Down Memoization

```kotlin
 fun climbStairsMemo(n: Int): Int {
        val memo = IntArray(n + 1) { -1 }
        return climbStairsHelper(n, memo)
    }
    
  private fun climbStairsHelper(n: Int, memo: IntArray): Int {
      if (n <= 2) return n
      
      if (memo[n] != -1) return memo[n]
      
      memo[n] = climbStairsHelper(n - 1, memo) + climbStairsHelper(n - 2, memo)
      return memo[n]
  }
```


## Approach 4: Mathematical (Fibonacci Formula - Advanced)

```kotlin
fun climbStairsMath(n: Int): Int {
        val sqrt5 = kotlin.math.sqrt(5.0)
        val phi = (1 + sqrt5) / 2
        val psi = (1 - sqrt5) / 2
        
        // nth Fibonacci number formula
        return ((kotlin.math.pow(phi, n + 1.0) - kotlin.math.pow(psi, n + 1.0)) / sqrt5).toInt()
    }
```
    
  
