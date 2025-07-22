# Kth Largest Element in a Stream
[Leetcode Problem](https://leetcode.com/problems/kth-largest-element-in-a-stream/)

## Problem Description
ou are part of a university admissions office and need to keep track of the kth highest test score from applicants in real-time. This helps to determine cut-off marks for interviews and admissions dynamically as new applicants submit their scores.

You are tasked to implement a class which, for a given integer k, maintains a stream of test scores and continuously returns the kth highest test score after a new score has been submitted. More specifically, we are looking for the kth highest score in the sorted list of all scores.

Implement the KthLargest class:
- KthLargest(int k, int[] nums) Initializes the object with the integer k and the stream of test scores nums.
- int add(int val) Adds a new test score val to the stream and returns the element representing the kth largest element in the pool of test scores so far.

## Examples

```kotlin
val kthLargest = KthLargest(3, intArrayOf(4, 5, 8, 2))
println(kthLargest.add(3))   // return 4
println(kthLargest.add(5))   // return 5  
println(kthLargest.add(10))  // return 5
println(kthLargest.add(9))   // return 8
println(kthLargest.add(4))   // return 8
```

## Constraints
- 1 <= k <= 10^4
- 0 <= nums.length <= 10^4
- 1 <= nums[i] <= 10^4
- 1 <= val <= 10^4
- At most 10^4 calls will be made to add

## Key Insights
- We need to maintain the K largest elements efficiently
- We don't need all elements sorted, just access to the Kth largest
- **Min Heap of size K** is perfect: the root is always the Kth largest element
- When heap is full and new element is larger than root, replace the root

## Solution Approach
1. **Use a Min Heap** with capacity K to store the K largest elements
2. **Heap property**: The smallest element in our K largest elements is at the root
3. **Add logic**:
   - **If heap size < K:** add element directly
   - **If new element > root:** remove root, add new element
   - **If new element ≤ root:** ignore (element is too small)
4. **Return**: Always return `heap.peek()` which is the Kth largest

## Complexity Analysis
- Time Complexity:
  - Constructor: O(n log k) where n = nums.length
  - add(): O(log k)
  
- Space Complexity: O(k)

## Solution

```kotlin
class KthLargest(k: Int, nums: IntArray) {
    private val capacity = k
    private val heap = PriorityQueue<Int>()
    
    init {
        for(num in nums) add(num)
    }
    
    fun add(`val`: Int): Int {
        if(heap.size < capacity) {
            heap.add(`val`)
        } else if(`val` > heap.peek()) {
            heap.remove()
            heap.add(`val`)
        }
        
        return heap.peek()
    }
}
```
