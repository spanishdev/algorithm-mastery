# Valid Perfect Square
[Leetcode Problem](https://leetcode.com/problems/valid-perfect-square/description/)

## Problem Description
Given a positive integer `num`, return `true` if `num` is a perfect square or `false` otherwise.
**A perfect square** is an integer that is the square of an integer. In other words, it is the product of some integer with itself.
You must not use any built-in library function, such as sqrt.

## Examples

```kotlin
Input: num = 16
Output: true
Explanation: 4 * 4 = 16 and 4 is an integer.

Input: num = 14
Output: false
Explanation: 3.742... * 3.742... = 14 but 3.742... is not an integer.

```

## Constraints
- 1 <= num <= 2^31 - 1

## Key Insights
- Search space: 1 to num (or optimized to num/2)
- Target: Find integer x where x * x == num
- Range-based binary search instead of array-based

## Solution Approach
- Search range: 1 to num
- Calculate: mid * mid for each candidate
- Compare: square with target number
- Eliminate: half of search space based on comparison

## Complexity Analysis
- Time: O(log n) - Binary search on range 1 to n
- Space: O(1) - Only using primitive variables

### Solution

```kotlin
fun isPerfectSquare(num: Int): Boolean {
    var start = 1
    var end = num
    
    while (start <= end) {
        val mid = (start + end) / 2
        val square = mid * mid
        
        when {
            square == num -> return true
            square > num -> end = mid - 1
            square < num -> start = mid + 1
        }
    }
    
    return false
}

```
