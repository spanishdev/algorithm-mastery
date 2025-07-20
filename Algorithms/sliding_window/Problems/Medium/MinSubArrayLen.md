# Problem
[Leetcode Problem](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Problem Description
Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.


## Examples

```kotlin
Example 1:
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.

Example 2:
Input: target = 4, nums = [1,4,4]
Output: 1

Example 3:
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0

```

## Constraints
- 1 <= target <= 10^9
- 1 <= nums.length <= 10^5
- 1 <= nums[i] <= 10^4

## Key Insights
- Use sliding window technique with variable window size
- Expand window by adding elements until sum >= target
- Once valid window found, try to shrink it from left while maintaining sum >= target
- Track minimum window size that satisfies the condition

## Solution Approach
- Use two pointers (left, right) for sliding window
- Expand window by moving right pointer and adding to current sum
- When sum >= target, try to shrink window from left
- Track minimum window size that satisfies the condition
- Return minimum size found, or 0 if no valid window exists

## Complexity Analysis
- Time Complexity: O(n) where n is the length of nums array
- Space Complexity: O(1) constant extra space

## Solution

```kotlin
fun minSubArrayLen(target: Int, nums: IntArray): Int {
        var result = Int.MAX_VALUE
        var sum = 0
        var start = 0

        for(end in nums.indices) {
            sum += nums[end]

            while(sum >= target) {
                var size = end - start + 1
                result = minOf(result, size)
                sum -= nums[start]
                start++
            }
        }

        return if(result == Int.MAX_VALUE) 0 else result
    }

```


