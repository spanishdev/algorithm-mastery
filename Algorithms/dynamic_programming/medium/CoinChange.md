# Coin Change
[Leetcode Problem](https://leetcode.com/problems/coin-change/description/)

## Problem Description
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

## Examples

```kotlin
Example 1:

Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1

Example 2:

Input: coins = [2], amount = 3
Output: -1

Example 3:

Input: coins = [1], amount = 0
Output: 0

 

```

## Constraints
- 1 <= coins.length <= 12
- 1 <= coins[i] <= 231 - 1
- 0 <= amount <= 104

## Key Insights
### Problem Analysis:
- Optimization problem: Find minimum number of coins
- Unlimited coins: Can use same coin multiple times
- Unbounded knapsack variant: Classic DP pattern

### DP State Definition:
dp[i] = minimum number of coins needed to make amount i

### Recurrence Relation:
For each amount i and each coin c:
- dp[i] = min(dp[i], dp[i - c] + 1) if i >= c

### Key Insight:
To make amount i, try using each coin c and see:
- If i >= c: Can we make i-c with fewer coins?
- Take minimum across all coin choices

### Bottom-up approach:
Build solutions from amount 0 up to target amount

## Solution Approach

### Approach 1: Bottom-Up DP (Most Popular)
Create dp array where dp[i] = min coins for amount i
For each amount, try each coin and take minimum
O(amount × coins.length) time

### Approach 2: Top-Down Memoization
Recursive with caching
Same time complexity but with recursion overhead

### Approach 3: BFS Approach
Treat as shortest path problem
Each level represents using one more coin
Less intuitive but interesting alternative


## Complexity Analysis

### Bottom-Up DP:
Time: O(amount × coins.length) - for each amount, try each coin
Space: O(amount) - DP array

### Top-Down Memoization:
Time: O(amount × coins.length) - each subproblem solved once
Space: O(amount) - recursion stack + memo

### BFS:
Time: O(amount × coins.length) - similar to DP
Space: O(amount) - queue storage

## Solutions

### Approach 1: Bottom-Up DP (Most Popular)

```kotlin
    fun coinChange(coins: IntArray, amount: Int): Int {
        if (amount == 0) return 0
        
        // dp[i] = minimum coins needed to make amount i
        val dp = IntArray(amount + 1) { amount + 1 }  // Initialize with impossible value
        dp[0] = 0  // Base case: 0 coins needed for amount 0
        
        // For each amount from 1 to target
        for (i in 1..amount) {
            // Try each coin
            for (coin in coins) {
                if (coin <= i) {
                    dp[i] = minOf(dp[i], dp[i - coin] + 1)
                }
            }
        }
        
        return if (dp[amount] > amount) -1 else dp[amount]
    }

```

### Approach 2: Top-Down Memoization

```kotlin
    fun coinChangeMemo(coins: IntArray, amount: Int): Int {
        val memo = IntArray(amount + 1) { -2 }  // -2 = not computed, -1 = impossible
        return coinChangeHelper(coins, amount, memo)
    }
    
    private fun coinChangeHelper(coins: IntArray, amount: Int, memo: IntArray): Int {
        if (amount == 0) return 0
        if (amount < 0) return -1
        
        if (memo[amount] != -2) return memo[amount]
        
        var min = Int.MAX_VALUE
        for (coin in coins) {
            val result = coinChangeHelper(coins, amount - coin, memo)
            if (result >= 0 && result < min) {
                min = result + 1
            }
        }
        
        memo[amount] = if (min == Int.MAX_VALUE) -1 else min
        return memo[amount]
    }

```


### Approach 3: BFS (Shortest Path)

```kotlin
    fun coinChangeBFS(coins: IntArray, amount: Int): Int {
        if (amount == 0) return 0
        
        val visited = BooleanArray(amount + 1)
        val queue = ArrayDeque<Int>()
        queue.offer(0)
        visited[0] = true
        
        var level = 0
        
        while (queue.isNotEmpty()) {
            level++
            val size = queue.size
            
            repeat(size) {
                val current = queue.poll()
                
                for (coin in coins) {
                    val next = current + coin
                    
                    if (next == amount) return level
                    
                    if (next < amount && !visited[next]) {
                        visited[next] = true
                        queue.offer(next)
                    }
                }
            }
        }
        
        return -1
    }

```

### Approach 4: Greedy*
* Only valid for normal currencies, not for this problem at all


```kotlin
fun coinChangeGreedy(coins: IntArray, amount: Int): Int {
    val sortedCoins = coins.sortedDescending()  // [4, 3, 1]
    var remaining = amount
    var count = 0
    
    for (coin in sortedCoins) {
        while (remaining >= coin) {
            remaining -= coin
            count++
        }
    }
    
    return if (remaining == 0) count else -1
}
```
