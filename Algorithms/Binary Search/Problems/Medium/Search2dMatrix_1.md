# Search a 2D Matrix

[Leetcode Problem](https://leetcode.com/problems/search-a-2d-matrix/description/)

## Problem Description
You are given an m x n integer matrix matrix with the following two properties:
* Each row is sorted in non-decreasing order.
* The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` if `target` is in matrix or `false` otherwise.

You must write a solution in O(log(m * n)) time complexity.
 
## Examples

```kotlin
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true

Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

## Constraints:
* m == matrix.length
* n == matrix[i].length
* 1 <= m, n <= 100
* -104 <= matrix[i][j], target <= 104

### Key Insights
- Matrix properties create one big sorted sequence
- Can treat 2D matrix as flattened 1D array
- Two-phase approach: Find correct row, then find element in row

### Solution Approach

**Two-phase binary search:**
- Find target row using range comparison
- Standard binary search within found row

**Complexity Analysis**
- Time: O(log n) - Three separate binary searches
- Space: O(1) - Only using primitive variables

### Solution

```kotlin
fun searchMatrix(matrix: Array<IntArray>, target: Int): Boolean {
    var row = 0
    var start = 0
    var end = matrix.size - 1
    
    // Phase 1: Find row
    while(start <= end) {
        val current = (start+end)/2
        val rangeScore = compareToRange(matrix[current], target)
        
        when {
            rangeScore == 0 -> {
                row = current
                break
            }
            rangeScore == -1 -> end = current - 1
            rangeScore == 1 -> start = current + 1
        }
    }
    
    start = 0
    end = matrix[row].size - 1
    
    // Phase 2: Find target
    while(start <= end) {
        val mid = (start+end)/2
        val midVal = matrix[row][mid]
        when {
            midVal == target -> return true
            midVal < target -> start = mid + 1
            midVal > target -> end = mid - 1
        }
    }
    
    return false
}

// Helper function: 0 in range, 1 greater than range, -1 less than range
fun compareToRange(array: IntArray, target: Int): Int {
    return when {
        target < array[0] -> -1
        target > array[array.size-1] -> 1
        else -> 0
    }
}
```
