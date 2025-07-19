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

### Solution Approach (Your Implementation)

**Three-phase algorithm:**
1. **Find any occurrence** of target (standard binary search)
2. **Find leftmost occurrence** (search from start to found index)
3. **Find rightmost occurrence** (search from found index to end)

**Complexity Analysis**
Time: O(log n) - Three separate binary searches
Space: O(1) - Only using primitive variables

### Your Code Solution
```kotlin
fun intervalElement(nums: IntArray, target: Int): List<Int> {
    var start = 0
    var end = nums.size - 1
    var targetIndex = 0
    
    // Phase 1: Find any target index
    while(start <= end) {
        targetIndex = (start+end)/2
        when {
            nums[targetIndex] == target -> break
            nums[targetIndex] < target -> start = targetIndex + 1
            nums[targetIndex] > target -> end = targetIndex - 1
        }
    }
    
    var first = -1
    var last = -1
    
    if(nums[targetIndex] == target) {
        // Phase 2: Search for first occurrence
        start = 0 
        end = targetIndex
        
        while(start <= end) {
            val mid = (start+end)/2
            when {
                nums[mid] == target -> {
                    first = mid
                    end = mid - 1
                }
                nums[mid] < target -> start = mid + 1
            }
        }
        
        // Phase 3: Search for last occurrence  
        last = first
        start = targetIndex
        end = nums.size - 1
        
        while(start <= end) {
            val mid = (start+end)/2
            when {
                nums[mid] == target -> {
                    last = mid 
                    start = mid + 1
                }
                nums[mid] > target -> end = mid - 1
            }
        }
    }
    
    return listOf(first, last)
}
```

