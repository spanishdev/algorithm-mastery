# Maximum Sum Subarray of Size K
[Leetcode Problem](https://leetcode.com/problems/maximum-sum-subarray-of-size-k/)

## Problem Description
Given an array of integers `nums` and an integer `k`, find the maximum sum of any contiguous subarray of size `k`.

## Examples
```kotlin
Input: nums = [2, 1, 5, 1, 3, 2], k = 3
Output: 9
Explanation: Subarray [5, 1, 3] has sum = 9

Input: nums = [2, 3, 4, 1, 5], k = 2  
Output: 7
Explanation: Subarray [3, 4] has sum = 7

Input: nums = [1], k = 1
Output: 1
Explanation: Only one subarray [1] with sum = 1
```

## Constraints
- 1 <= k <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- Array can contain negative numbers
- k is always valid (never larger than array size)

## Key Insights
- Fixed window size - window always maintains exactly k elements
- Sliding optimization - instead of recalculating sum for each window, add new element and remove old element
- Single pass - traverse array once while maintaining window
- Brute force would be O(n*k), sliding window achieves O(n)

## Solution Approach
Fixed-size sliding window:

- Build window gradually from start until size k
- Track maximum sum throughout the process
- Maintain window size by removing leftmost element when window reaches size k
- Continue sliding until end of array

## Complexity Analysis
- Time: O(n) - Single pass through array
- Space: O(1) - Only using constant extra variables

## Solution
```kotlin
fun maxSumSubarray(nums: IntArray, k: Int): Int {
    var start = 0
    var end = 1
    var currentSum = nums[start]
    var maxSum = currentSum
    
    while (end < nums.size) {
        currentSum += nums[end]
        maxSum = maxOf(maxSum, currentSum)
        
        val windowSize = end - start + 1
        if (windowSize == k) {
            currentSum -= nums[start]
            start++
        }
        
        end++
    }
    
    return maxSum
}
```
