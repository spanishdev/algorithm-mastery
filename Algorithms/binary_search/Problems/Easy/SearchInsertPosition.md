# Search Insert Position
[Leetcode Problem](https://leetcode.com/problems/search-insert-position/description/)

## Problem Description
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
You must write an algorithm with O(log n) runtime complexity.

## Examples
```kotlin
Input: nums = [1,3,5,6], target = 5
Output: 2

Input: nums = [1,3,5,6], target = 2
Output: 1

Input: nums = [1,3,5,6], target = 7
Output: 4

Input: nums = [1,3,5,6], target = 0
Output: 0
```

## Constraints
- 1 <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- nums contains distinct values sorted in ascending order
- -10^4 <= target <= 10^4

### Key Insights
- Always returns valid index (never -1)
- Binary search naturally finds insertion point when element not found
- start pointer converges to insertion position when loop terminates

### Solution Approach
Modified binary search:
- Standard binary search for target
- If found: return index directly
- If not found: return start pointer (insertion position)

Complexity Analysis
- Time: O(log n) - Standard binary search
- Space: O(1) - Only using primitive variables

### Solution
```kotlin
fun searchInsert(nums: IntArray, target: Int): Int {
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
    
    return start
}
```
