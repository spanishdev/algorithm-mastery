# Problem
[Leetcode Problem](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/)

## Problem Description
Given the root of a Binary Search Tree and a target number k, return true if there exist two elements in the BST such that their sum is equal to the given target.

## Examples

![sum_tree_1](https://github.com/user-attachments/assets/086b2176-98a8-412a-abf0-4e7edd0c5b60)

```kotlin
Input: root = [5,3,6,2,4,null,7], k = 9
Output: true

```

![sum_tree_2](https://github.com/user-attachments/assets/a65abb25-df26-4522-ae38-61067c5c8c54)

```kotlin
Input: root = [5,3,6,2,4,null,7], k = 28
Output: false

```

## Constraints
- The number of nodes in the tree is in the range [1, 104].
- -104 <= Node.val <= 104
- root is guaranteed to be a valid binary search tree.
- -105 <= k <= 105


## Key Insights
### Classic Two Sum Pattern:
- For each node value x, check if k - x exists in the tree
- Similar to Two Sum in array, but with BST structure

### Multiple Approaches:
- HashSet approach - Store seen values, check complements
- BST search approach - For each node, search for complement in BST
- Two pointers on inorder - Convert to sorted array, use two pointers
- Inorder + binary search - Traverse + search for complement

### Trade-offs:
- HashSet: O(n) time, O(n) space, simple
- BST search: O(n log n) time, O(H) space, uses BST properties
- Two pointers: O(n) time, O(n) space, classic technique

## Solution Approach
### Approach 1: HashSet (Most Popular)
Traverse tree and maintain HashSet of seen values
For each node, check if k - node.val exists in set

### Approach 2: BST Search for Complement
For each node, search the BST for k - node.val
Avoid counting the same node twice

### Approach 3: Two Pointers on Inorder Array
Convert BST to sorted array using inorder traversal
Apply classic two-pointer technique

### Approach 4: Iterator-based Two Pointers
Create forward and backward iterators
Simulate two pointers without creating array

## Complexity Analysis

###  HashSet Approach
- Time: O(n) - single traversal
- Space: O(n) - HashSet storage

### BST Search Approach
- Time: O(n log n) - n nodes × log n search each
- Space: O(H) - recursion stack only

### Two Pointers on Array
- Time: O(n) - inorder + two pointers
- Space: O(n) - array storage + O(H) recursion

### Iterator-based Two Pointers
- Time: O(n) - each node visited at most twice
- Space: O(H) - iterator stacks

## Solution

### Definition TreeNode

```kotlin
/**
 * Definition for a binary tree node.
 */
class TreeNode(var `val`: Int) {
    var left: TreeNode? = null
    var right: TreeNode? = null
}

```

### Approach 1: HashSet (Most Popular & Efficient)

```kotlin
 fun findTarget(root: TreeNode?, k: Int): Boolean {
        val seen = mutableSetOf<Int>()
        return findTargetHelper(root, k, seen)
    }
    
    private fun findTargetHelper(node: TreeNode?, k: Int, seen: MutableSet<Int>): Boolean {
        if (node == null) return false
        
        val complement = k - node.`val`
        if (complement in seen) return true
        
        seen.add(node.`val`)
        
        return findTargetHelper(node.left, k, seen) || 
               findTargetHelper(node.right, k, seen)
    }

```

### Approach 2: BST Search for Complement

```kotlin
  fun findTargetBSTSearch(root: TreeNode?, k: Int): Boolean {
        return searchPair(root, root, k)
    }
    
    private fun searchPair(root: TreeNode?, node: TreeNode?, k: Int): Boolean {
        if (node == null) return false
        
        val complement = k - node.`val`
        
        // Search for complement in BST (but not the same node)
        if (complement != node.`val` && searchBST(root, complement)) {
            return true
        }
        
        return searchPair(root, node.left, k) || 
               searchPair(root, node.right, k)
    }
    
    private fun searchBST(node: TreeNode?, target: Int): Boolean {
        if (node == null) return false
        
        return when {
            target < node.`val` -> searchBST(node.left, target)
            target > node.`val` -> searchBST(node.right, target)
            else -> true
        }
    }

```

### Approach 3: Two Pointers on Inorder Array

```kotlin
 fun findTargetTwoPointers(root: TreeNode?, k: Int): Boolean {
        val inorderList = mutableListOf<Int>()
        inorderTraversal(root, inorderList)
        
        var left = 0
        var right = inorderList.size - 1
        
        while (left < right) {
            val sum = inorderList[left] + inorderList[right]
            when {
                sum == k -> return true
                sum < k -> left++
                else -> right--
            }
        }
        
        return false
    }
    
    private fun inorderTraversal(node: TreeNode?, list: MutableList<Int>) {
        if (node == null) return
        
        inorderTraversal(node.left, list)
        list.add(node.`val`)
        inorderTraversal(node.right, list)
    }

```

### Approach 4: Iterator-based Two Pointers (Advanced)

```kotlin
 fun findTargetIterators(root: TreeNode?, k: Int): Boolean {
        val leftIterator = BSTIterator(root, true)   // Forward iterator
        val rightIterator = BSTIterator(root, false) // Backward iterator
        
        var left = leftIterator.next()
        var right = rightIterator.next()
        
        while (left < right) {
            val sum = left + right
            when {
                sum == k -> return true
                sum < k -> left = leftIterator.next()
                else -> right = rightIterator.next()
            }
        }
        
        return false
    }
    
    // Helper class for iterator-based approach
    class BSTIterator(root: TreeNode?, private val forward: Boolean) {
        private val stack = mutableListOf<TreeNode>()
        
        init {
            pushAll(root)
        }
        
        fun next(): Int {
            val node = stack.removeAt(stack.size - 1)
            if (forward) {
                pushAll(node.right)
            } else {
                pushAll(node.left)
            }
            return node.`val`
        }
        
        private fun pushAll(node: TreeNode?) {
            var current = node
            while (current != null) {
                stack.add(current)
                current = if (forward) current.left else current.right
            }
        }
    }
```

### Approach 5: Clean HashSet with Early Return

```kotlin
 fun findTargetClean(root: TreeNode?, k: Int): Boolean {
        val seen = mutableSetOf<Int>()
        
        fun dfs(node: TreeNode?): Boolean {
            if (node == null) return false
            
            if ((k - node.`val`) in seen) return true
            seen.add(node.`val`)
            
            return dfs(node.left) || dfs(node.right)
        }
        
        return dfs(root)
    }
```
