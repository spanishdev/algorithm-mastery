# Merge k Sorted Lists
[Leetcode Problem](https://leetcode.com/problems/merge-k-sorted-lists/description/)

## Problem Description
You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.
Merge all the linked-lists into one sorted linked-list and return it.

## Examples
  
### Example 1
```  
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted linked list:
1->1->2->3->4->4->5->6
```  
###  Example 2

```  
Input: lists = []
Output: []
```

###  Example 3:

```
Input: lists = [[]]
Output: []
```

## Constraints
- k == lists.length
- 0 <= k <= 104
- 0 <= lists[i].length <= 500
- -104 <= lists[i][j] <= 104
- lists[i] is sorted in ascending order.
- The sum of lists[i].length will not exceed 104.

## Key Insights
### Problem Analysis:

Multiple sorted lists → need to merge efficiently
Output should be a single sorted linked list
Each input list is already sorted (advantage!)

### Core Challenge:
How to efficiently find the "next smallest" element across k lists?

### Approaches:

Brute Force - Compare all heads, pick smallest (O(k) per element)
Priority Queue/Heap - Min-heap to track smallest heads (O(log k) per element)
Divide & Conquer - Pair-wise merging (optimal for large k)
Sequential merging - Merge lists one by one

### Scalability:
Small k (2-10): Any approach works
Large k (100+): Heap or Divide & Conquer much better

## Solution Approach

### Approach 1: Min-Heap/Priority Queue (Most Popular)
Put first node of each list in min-heap
Extract minimum, add its next node to heap
Repeat until heap is empty

### Approach 2: Divide & Conquer (Most Efficient)
Merge lists pair-wise: merge(0,1), merge(2,3), etc.
Repeat until only one list remains
Optimal time complexity


## Complexity Analysis
### Min-Heap Approach:
- Time: O(n log k) where n = total nodes, k = number of lists
- Space: O(k) for the heap

### Divide & Conquer:
- Time: O(n log k) - most efficient
- Space: O(log k) for recursion stack


## Solution

```kotlin
/**
 * Definition for singly-linked list.
 */
class ListNode(var `val`: Int) {
    var next: ListNode? = null
}

```

### Approach 1

```kotlin
  fun mergeKLists(lists: Array<ListNode?>): ListNode? {
        val heap = PriorityQueue<ListNode> { a, b -> a.`val` - b.`val` }
       
        for (node in lists) node?.let(heap::add)
        if (heap.isEmpty()) return null
        
        val dummyNode = ListNode(0)
        var current = dummyNode
        
        while (!heap.isEmpty()) {
            val minNode = heap.poll()
            current.next = minNode
            current = current.next!!
            minNode.next?.let { heap::add }
        }
        
        return dummyNode.next
    }
```

### Approach 2

```kotlin
fun mergeKListsDivideConquer(lists: Array<ListNode?>): ListNode? {
        if (lists.isEmpty()) return null
        if (lists.size == 1) return lists[0]
        
        return mergeKListsHelper(lists, 0, lists.size - 1)
    }
    
  private fun mergeKListsHelper(lists: Array<ListNode?>, start: Int, end: Int): ListNode? {
      if (start == end) return lists[start]
      if (start > end) return null
      
      val mid = start + (end - start) / 2
      val left = mergeKListsHelper(lists, start, mid)
      val right = mergeKListsHelper(lists, mid + 1, end)
      
      return mergeTwoLists(left, right)
  }
```
