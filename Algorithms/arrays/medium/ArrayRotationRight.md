# Rotate array right
[Leetcode Problem]()

## Problem Description
Given an array, rotate the array to the right by k steps, where k is non-negative.

## Examples

```kotlin
Example 1:
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

Example 2:
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

```

## Constraints
- 1 <= nums.length <= 10^5
- -2^31 <= nums[i] <= 2^31 - 1
- 0 <= k <= 10^5
- 
Follow up: Try to come up with as many solutions as you can. There are at least three different ways to solve this problem.
Follow up: Could you do it in-place with O(1) extra memory?

## Key Insights
- When k >= n, only k % n rotations matter (due to cyclic nature)
- Multiple approaches: reverse technique, extra space, cyclic replacement
- Reverse technique is most elegant: reverse entire array, then reverse parts
- In-place solutions are more memory efficient

## Solution Approach
Reverse Technique (Optimal):

- Handle edge case: k = k % n to avoid unnecessary rotations
- Reverse the entire array
- Reverse the first k elements
- Reverse the remaining n-k elements

Example: 

```kotlin
nums = [1,2,3,4,5,6,7], k = 3

Step 1: k = 3 % 7 = 3
Step 2: Reverse entire → [7,6,5,4,3,2,1]
Step 3: Reverse first 3 → [5,6,7,4,3,2,1]
Step 4: Reverse last 4 → [5,6,7,1,2,3,4] ✅
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
    reverse(nums, 0, n - 1)           // Reverse entire array
    reverse(nums, 0, steps - 1)       // Reverse first k elements  
    reverse(nums, steps, n - 1)       // Reverse remaining elements
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
