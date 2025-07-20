# Find First and Last Position of Element in Sorted Array

[Leetcode Problem](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

## Problem Description
Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value. 
If `target` is not found, return `[-1, -1]`. 
Must achieve O(log n) runtime complexity.

## Examples
```kotlin
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

Input: nums = [1], target = 1
Output: [0,0]
```

### Key Insights
- Need to find **range boundaries** (first and last occurrence)
- Single occurrence returns `[index, index]`
- Requires **multiple binary searches** or **modified binary search logic**

### Solution Approach

**Two-phase algorithm:**
1. **Find leftmost occurrence** 
2. **Find rightmost occurrence**

**Complexity Analysis**
Time: O(log n) - Three separate binary searches
Space: O(1) - Only using primitive variables

### Solution

```kotlin
fun searchRange(nums: IntArray, target: Int): IntArray {
    val first = findFirst(nums, target)
    if (first == -1) return intArrayOf(-1, -1)
    
    val last = findLast(nums, target)
    return intArrayOf(first, last)
}

// Find leftmost occurrence
fun findFirst(nums: IntArray, target: Int): Int {
    var start = 0
    var end = nums.size - 1
    var result = -1
    
    while (start <= end) {
        val mid = (start + end) / 2
        when {
            nums[mid] == target -> {
                result = mid      // Found it, but keep searching LEFT
                end = mid - 1     // Continue searching left half
            }
            nums[mid] < target -> start = mid + 1
            nums[mid] > target -> end = mid - 1
        }
    }
    
    return result
}

// Find rightmost occurrence  
fun findLast(nums: IntArray, target: Int): Int {
    var start = 0
    var end = nums.size - 1
    var result = -1
    
    while (start <= end) {
        val mid = (start + end) / 2
        when {
            nums[mid] == target -> {
                result = mid      // Found it, but keep searching RIGHT
                start = mid + 1   // Continue searching right half
            }
            nums[mid] < target -> start = mid + 1
            nums[mid] > target -> end = mid - 1
        }
    }
    
    return result
}
```

