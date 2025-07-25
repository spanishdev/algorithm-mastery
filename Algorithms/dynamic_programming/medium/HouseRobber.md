# House Robber
[Leetcode Problem](https://leetcode.com/problems/house-robber/description/)

## Problem Description
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

## Examples

```kotlin
Example 1:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.

```

## Constraints
- 1 <= nums.length <= 100
- 0 <= nums[i] <= 400

## Key Insights

### Problem Analysis:
Cannot rob two adjacent houses
Want to maximize total money stolen
Classic optimization problem with constraints

### DP State Definition:
For each house i, you have two choices:
- Rob house i: Get nums[i] + max_money_up_to(i-2)
- Skip house i: Get max_money_up_to(i-1)

### Recurrence Relation:
dp[i] = max(nums[i] + dp[i-2], dp[i-1])

### Key Insight:
At each house, choose the maximum between:
- Taking current house + best result from 2 houses ago
- Skipping current house + best result from previous house

## Solution Approach

## Approach 1: Space Optimized (Best)
Only track previous 2 values
O(n) time, O(1) space

## Approach 2: Bottom-Up Tabulation
Fill DP array from left to right
O(n) time, O(n) space

## Approach 3: Top-Down Memoization
Recursive with caching
O(n) time, O(n) space


## Complexity Analysis

### Space Optimized:
Time: O(n) - single pass through array
Space: O(1) - only 2 variables

### Tabulation:
Time: O(n) - fill array once
Space: O(n) - DP array

### Memoization:
Time: O(n) - each house considered once
Space: O(n) - recursion stack + memo


## Approach 1: Space Optimized (Best for interviews)

```kotlin
    fun rob(nums: IntArray): Int {
        if (nums.isEmpty()) return 0
        if (nums.size == 1) return nums[0]
        
        var prev2 = 0          // max money up to house i-2
        var prev1 = nums[0]    // max money up to house i-1
        
        for (i in 1 until nums.size) {
            val current = maxOf(nums[i] + prev2, prev1)
            prev2 = prev1
            prev1 = current
        }
        
        return prev1
    }

```

## Approach 2: Bottom-Up Tabulation (Most intuitive)

```kotlin
  fun robTabulation(nums: IntArray): Int {
        if (nums.isEmpty()) return 0
        if (nums.size == 1) return nums[0]
        
        val dp = IntArray(nums.size)
        dp[0] = nums[0]
        dp[1] = maxOf(nums[0], nums[1])
        
        for (i in 2 until nums.size) {
            dp[i] = maxOf(nums[i] + dp[i - 2], dp[i - 1])
        }
        
        return dp[nums.size - 1]
    }

```
## Approach 3: Top-Down Memoization

```kotlin
 fun robMemo(nums: IntArray): Int {
        if (nums.isEmpty()) return 0
        val memo = IntArray(nums.size) { -1 }
        return robHelper(nums, nums.size - 1, memo)
    }
    
    private fun robHelper(nums: IntArray, i: Int, memo: IntArray): Int {
        if (i < 0) return 0
        if (i == 0) return nums[0]
        
        if (memo[i] != -1) return memo[i]
        
        memo[i] = maxOf(
            nums[i] + robHelper(nums, i - 2, memo),  // Rob current house
            robHelper(nums, i - 1, memo)             // Skip current house
        )
        
        return memo[i]
    }

```


