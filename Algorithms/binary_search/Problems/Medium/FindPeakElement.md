# Find Peak Element
[Leetcode Problem](https://leetcode.com/problems/find-peak-element/description/)

## Problem Description
A peak element is an element that is strictly greater than its neighbors.
Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any** of the peaks.
You may imagine that `nums[-1] = nums[n] = -∞` (negative infinity).
You must write an algorithm that runs in **O(log n)** time.

## Examples
```kotlin
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return index 2.

Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index 1 or 5.
```

## Constraints:
- 1 <= nums.length <= 1000
- -2^31 <= nums[i] <= 2^31 - 1
- nums[i] != nums[i + 1] for all valid i

## Key Insights
- Any peak is valid - don't need to find specific one
- Follow the slope - if going upward, peak is ahead
- Boundary guarantee - nums[-1] = nums[n] = -∞ ensures peak exists
- No need to detect exact peak - just follow gradient direction

## Solution Approach
Single binary search with gradient following:

**Compare nums[mid] with nums[mid + 1]**
- If ascending → search right half (peak ahead)
- If descending → search left half (peak is here or behind)
- Algorithm guarantees convergence to peak

## Complexity Analysis
- Time: O(log n) - Single binary search
- Space: O(1) - Only using primitive variables

## Solution
```kotlin
fun findPeak(nums: IntArray): Int {
    var start = 0
    var end = nums.size - 1
    
    while (start < end) {
        val mid = (start + end) / 2
        
        if (nums[mid] < nums[mid + 1]) {
            start = mid + 1  // Peak is to the right
        } else {
            end = mid        // Peak is at mid or to the left
        }
    }
    
    return start
}
```
