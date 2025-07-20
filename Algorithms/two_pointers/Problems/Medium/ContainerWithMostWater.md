
# Container With Most Water
[Leetcode Problem]()

## Problem Description
You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).
Find two lines that together with the x-axis form a container, such that the container contains the most water.
Return the maximum amount of water a container can store.
Note: You may not slant the container.

## Examples

```kotlin
Example 1:
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

Example 2:
Input: height = [1,1]
Output: 1

```

## Constraints
- n == height.length
- 2 <= n <= 10^5
- 0 <= height[i] <= 10^4

## Key Insights
- Use two pointers technique starting from both ends
- Area = min(height[left], height[right]) * (right - left)
- Always move the pointer with smaller height (greedy approach)
- Moving the taller pointer won't increase area with current shorter one

## Solution Approach
- Start with two pointers at the beginning and end of array
- Calculate current area using the shorter height and distance
- Move the pointer with the smaller height inward
- Keep track of maximum area found
- Continue until pointers meet

## Complexity Analysis
- Time Complexity: O(n) where n is the length of height array
- Space Complexity: O(1) constant extra space

## Solution

```kotlin
fun maxArea(height: IntArray): Int {
    var left = 0
    var right = height.size - 1
    var maxWater = 0
    
    while (left < right) {
        val currentArea = minOf(height[left], height[right]) * (right - left)
        maxWater = maxOf(maxWater, currentArea)
        
        if (height[left] < height[right]) {
            left++
        } else {
            right--
        }
    }
    
    return maxWater
}
```
