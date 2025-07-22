# Kth Largest Element in an Array
[Leetcode Problem](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

## Problem Description
Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

Can you solve it without sorting?

 

## Examples

```kotlin
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5

Input: nums = [3,2,3,1,2,4,5,5,6], k = 4  
Output: 4

```

## Constraints
- 1 <= k <= nums.length <= 10^5
- -10^4 <= nums[i] <= 10^4

## Key Insights
- Similar to the stream problem, but we have all elements at once
- Use Min Heap of size K to maintain the K largest elements
- The root of the heap will always be the Kth largest element
- Process all elements in one pass through the array

## Solution Approach
- Use Min Heap with capacity K
- For each element in nums:
  -If heap size < K: add element directly
  -Else if element > heap root: replace root with element
  -Else: ignore element (too small)
- Return: heap.peek() which is the Kth largest

## Complexity Analysis
- Time Complexity: O(n log k) where n = nums.length
- Space Complexity: O(k)

## Solution

```kotlin
fun findKthLargest(nums: IntArray, k: Int): Int {
    val heap = PriorityQueue<Int>()
   
    for(n in nums) {
        if(heap.size < k) {
            heap.add(n)
        } else if(n > heap.peek()) {
            heap.remove()
            heap.add(n)
        }
    }
   
    return heap.peek()
}

```
