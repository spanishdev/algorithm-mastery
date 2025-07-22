# Rotate array left
[Leetcode Problem]()

## Problem Description
Given an array, rotate the array to the left by k steps, where k is non-negative.

## Examples

```kotlin
Example 1:
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [4,5,6,7,1,2,3]
Explanation:
rotate 1 step to the left: [2,3,4,5,6,7,1]
rotate 2 steps to the left: [3,4,5,6,7,1,2]
rotate 3 steps to the left: [4,5,6,7,1,2,3]

Example 2:
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 step to the left: [-100,3,99,-1]
rotate 2 steps to the left: [3,99,-1,-100]

Example 3:
Input: nums = [1,2], k = 1
Output: [2,1]

```

## Constraints
- 1 <= nums.length <= 10^5
- -2^31 <= nums[i] <= 2^31 - 1
- 0 <= k <= 10^5
- 
Follow up: Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.
Follow up: Could you do it in-place with O(1) extra memory?

## Key Insights
- Left rotation by k is equivalent to right rotation by (n - k)
- When k >= n, only k % n rotations matter (due to cyclic nature)
- Multiple approaches: reverse technique, extra space, direct shifting
- Reverse technique: reverse first k, reverse rest, reverse entire

## Solution Approach
Reverse Technique (Optimal):

- Handle edge case: k = k % n to avoid unnecessary rotations
- Reverse the first k elements
- Reverse the remaining n-k elements
- Reverse the entire array

Example: 

```kotlin
nums = [1,2,3,4,5,6,7], k = 3

Step 1: k = 3 % 7 = 3
Step 2: Reverse first 3 → [3,2,1,4,5,6,7]
Step 3: Reverse last 4 → [3,2,1,7,6,5,4]
Step 4: Reverse entire → [4,5,6,7,1,2,3] ✅

```

## Complexity Analysis
- Time Complexity: O(n) where n is the length of array
- Space Complexity: O(1) constant extra space (in-place)

## Solution

```kotlin
fun rotate(nums: IntArray, k: Int): Unit {
    val n = nums.size
    val steps = k % n  // Handle k >= n
    
    if (steps == 0) return  // No rotation needed
    
    // Three-step reverse process
    reverse(nums, 0, steps - 1)       // Reverse first k elements
    reverse(nums, steps, n - 1)       // Reverse remaining elements
    reverse(nums, 0, n - 1)           // Reverse entire array
}

private fun reverse(nums: IntArray, start: Int, end: Int) {
  var left = start
  var right = end
  while (left < right) {
    val temp = nums[left]
    nums[left] = nums[right]
    nums[right] = temp
    left++
    right--
  }
}

```
