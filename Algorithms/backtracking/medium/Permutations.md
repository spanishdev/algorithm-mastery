# Permutations
[Leetcode Problem](https://leetcode.com/problems/permutations/description/)

## Problem Description
Given an array nums of distinct integers, return all the possible . You can return the answer in any order.

## Examples

```kotlin
Example 1:

Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

Example 2:

Input: nums = [0,1]
Output: [[0,1],[1,0]]

Example 3:

Input: nums = [1]
Output: [[1]]

```

## Constraints
- 1 <= nums.length <= 6
- -10 <= nums[i] <= 10
- All the integers of nums are unique.

## Key Insights

### Problem Analysis:
- Generate ALL possible arrangements of the given numbers
- Order matters: [1,2,3] ≠ [3,2,1]
- Each number used exactly once per permutation
- Total permutations = n! where n = nums.length

### Backtracking Approach:
- Choose a number from available options
- Explore permutations with that choice
- Unchoose (backtrack) and try next option

### Key Components:
- Base case: When permutation length equals input length
- Used tracking: Boolean array to mark used numbers
- Backtracking: Unmark numbers when backtracking

#### Decision Tree Example for [1,2,3]:

```
                    []
          /         |         \
       [1]        [2]        [3]
      /   \      /   \      /   \
   [1,2] [1,3] [2,1] [2,3] [3,1] [3,2]
    |     |     |     |     |     |
  [1,2,3][1,3,2][2,1,3][2,3,1][3,1,2][3,2,1]
   ✅     ✅     ✅     ✅     ✅     ✅
```

## Complexity Analysis

### Time Complexity: O(n! × n)
n! permutations to generate
O(n) time to copy each permutation to result

### Space Complexity: O(n! × n + n)
O(n! × n) to store all permutations
O(n) for recursion stack depth
O(n) for used array


## Solution

```kotlin
    fun permute(nums: IntArray): List<List<Int>> {
        val result: MutableList<List<Int>> = mutableListOf()
        val used = BooleanArray(nums.size)
        permute(nums, used, mutableListOf(), result)
        return result
    }
    
    fun permute(
        nums: IntArray,
        used: BooleanArray, 
        path: MutableList<Int>, 
        result: MutableList<List<Int>>
    ) {
        if (path.size == nums.size) {
            result.add(path.toList()) // Important to add a copy, or the list will mess up
            return
        }
        
        for (i in nums.indices) {
            if (!used[i]) {
                used[i] = true
                path.add(nums[i])
                permute(nums, used, path, result)
                path.removeLast()
                used[i] = false
            }
        }
    }
```
