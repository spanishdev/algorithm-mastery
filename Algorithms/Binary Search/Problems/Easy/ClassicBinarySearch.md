# Classic Binary Search

## Leetcode Problem
[Leetcode Problem](https://leetcode.com/problems/problems/binary-search/description/)

## Problem Description
Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.
You must write an algorithm with `O(log n)` runtime complexity.

## Examples
```kotlin
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
```

## Constraints
- 1 <= nums.length <= 10^4
- -10^4 <= nums[i], target <= 10^4
- All the integers in nums are unique
- nums is sorted in ascending order

## Key Insights
- Standard binary search template - foundation for all variations
- Sorted array allows elimination of half search space each iteration
- Unique elements simplifies comparison logic

## Solution Approach
- Standard binary search algorithm:
- Initialize start and end pointers
- Calculate middle index
- Compare middle element with target
- Eliminate half of search space based on comparison
- Repeat until found or search space exhausted

## Complexity Analysis
- Time: O(log n) - Eliminates half of search space each iteration
- Space: O(1) - Only using primitive variables

## Solution
```kotlin
fun search(nums: IntArray, target: Int): Int {
    var start = 0
    var end = nums.size - 1
    
    while (start <= end) {
        val mid = (start + end) / 2
        when {
            nums[mid] == target -> return mid
            nums[mid] > target -> end = mid - 1
            nums[mid] < target -> start = mid + 1
        }
    }
    
    return -1
}
```


