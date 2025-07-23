# Kth Smallest Element in a BST

[Leetcode Problem](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

## Problem Description
Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

## Examples

![kthtree1](https://github.com/user-attachments/assets/37c77bc2-d3e3-4f7a-9894-42d48933e504)

```kotlin
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

![kthtree2](https://github.com/user-attachments/assets/f0e8b7e4-c12c-41d8-bbbf-8068b3728177)

```kotlin
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3

```

## Constraints
- The number of nodes in the tree is n.
- 1 <= k <= n <= 104
- 0 <= Node.val <= 104


## Key Insights


## Solution Approach


## Complexity Analysis


## Solution

```kotlin
class Solution {
    fun kthSmallest(root: TreeNode?, k: Int): Int {
        val list = mutableListOf<Int>()
        kthSmallestRec(root, k, list)

        return list.last()
    }

    fun kthSmallestRec(node: TreeNode?, k: Int, list: MutableList<Int>) {
        if(node == null) return

        node?.left?.let{ kthSmallestRec(it, k, list) }

        if(list.size < k) list.add(node.`val`)

        if(list.size < k) node?.right?.let{ kthSmallestRec(it, k, list) }
    }
}

```


**Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

Using a cache, clearing the cache when inserting/deleting and recalculating again
