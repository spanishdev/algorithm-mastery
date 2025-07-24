# Range sum of BST submissions
[Leetcode Problem](https://leetcode.com/problems/range-sum-of-bst/submissions/1710060772/)

## Problem Description
Given the root node of a binary search tree and two integers low and high, return the sum of values of all nodes with a value in the inclusive range [low, high].

## Examples

![bst1](https://github.com/user-attachments/assets/2fb48657-e179-4536-9372-3e748a410654)

```kotlin
Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
Output: 32
Explanation: Nodes 7, 10, and 15 are in the range [7, 15]. 7 + 10 + 15 = 32.

```

![bst2](https://github.com/user-attachments/assets/d9efba99-6aa0-4178-9fe4-2bdba70869e5)


```kotlin
Input: root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
Output: 23
Explanation: Nodes 6, 7, and 10 are in the range [6, 10]. 6 + 7 + 10 = 23.

```

## Constraints
- The number of nodes in the tree is in the range [1, 2 * 104].
- 1 <= Node.val <= 105
- 1 <= low <= high <= 105
- All Node.val are unique.


## Key Insights
- If node.val < low → only right subtree can have valid values
- If node.val > high → only left subtree can have valid values
- If low <= node.val <= high → node is valid, check both subtrees

## Solution Approach
### Approach 1: Recursive with Pruning (Optimal)
- Use BST properties to avoid unnecessary traversals
- Only explore subtrees that might contain values in range
- 
### Approach 2: Inorder Traversal with Early Termination
- Perform inorder traversal
- Stop when we exceed the high value
- Skip values below low
- 
### Approach 3: Iterative with Stack
- Use explicit stack to avoid recursion
- Apply same pruning logic iteratively

## Complexity Analysis
### Recursive with Pruning:
- Time: O(log n + k) where k = number of nodes in range
- Space: O(H) for recursion stack, H = height of tree
- Best case: O(log n) when range is very specific
- 
### Inorder with Early Termination:
- Time: O(n) worst case, but can terminate early
- Space: O(H) for recursion stack

### Iterative:
- Time: O(log n + k)
- Space: O(H) for explicit stack

## Solution

Given the following TreeNode:

```kotlin
/**
 * Definition for a binary tree node.
 */
class TreeNode(var `val`: Int) {
    var left: TreeNode? = null
    var right: TreeNode? = null
}
```

### Approach 1: Recursive with Pruning (Recommended)

```kotlin
    fun rangeSumBST(root: TreeNode?, low: Int, high: Int): Int {
        if (root == null) return 0
        
        return when {
            // Current node is below range - only check right subtree
            root.`val` < low -> rangeSumBST(root.right, low, high)
            
            // Current node is above range - only check left subtree  
            root.`val` > high -> rangeSumBST(root.left, low, high)
            
            // Current node is in range - include it and check both subtrees
            else -> root.`val` + 
                   rangeSumBST(root.left, low, high) + 
                   rangeSumBST(root.right, low, high)
        }
    }
```

  ### Approach 2: Explicit if-else for clarity

```kotlin
   fun rangeSumBSTExplicit(root: TreeNode?, low: Int, high: Int): Int {
        if (root == null) return 0
        
        var sum = 0
        
        // If current node is in range, add it
        if (root.`val` >= low && root.`val` <= high) {
            sum += root.`val`
        }
        
        // Only explore left subtree if current node > low
        if (root.`val` > low) {
            sum += rangeSumBSTExplicit(root.left, low, high)
        }
        
        // Only explore right subtree if current node < high
        if (root.`val` < high) {
            sum += rangeSumBSTExplicit(root.right, low, high)
        }
        
        return sum
    }
```


### Approach 3: Inorder Traversal with Early Termination

```kotlin
  fun rangeSumBSTInorder(root: TreeNode?, low: Int, high: Int): Int {
        var sum = 0
        
        fun inorder(node: TreeNode?) {
            if (node == null) return
            
            // If current node > high, no need to go right (early termination)
            if (node.`val` > high) {
                inorder(node.left)
                return
            }
            
            // If current node < low, no need to go left
            if (node.`val` < low) {
                inorder(node.right)
                return
            }
            
            // Normal inorder: left -> node -> right
            inorder(node.left)
            
            if (node.`val` >= low && node.`val` <= high) {
                sum += node.`val`
            }
            
            inorder(node.right)
        }
        
        inorder(root)
        return sum
    }
```

### Approach 4: Iterative with Stack

```kotlin
fun rangeSumBSTIterative(root: TreeNode?, low: Int, high: Int): Int {
        if (root == null) return 0
        
        val stack = mutableListOf<TreeNode>()
        stack.add(root)
        var sum = 0
        
        while (stack.isNotEmpty()) {
            val node = stack.removeAt(stack.size - 1)
            
            when {
                node.`val` < low -> {
                    // Only add right child
                    node.right?.let { stack.add(it) }
                }
                node.`val` > high -> {
                    // Only add left child
                    node.left?.let { stack.add(it) }
                }
                else -> {
                    // Node is in range
                    sum += node.`val`
                    // Add both children
                    node.left?.let { stack.add(it) }
                    node.right?.let { stack.add(it) }
                }
            }
        }
        
        return sum
    }
```
  ### Approach 5: Collect nodes in range, then sum

```kotlin
 fun rangeSumBSTCollect(root: TreeNode?, low: Int, high: Int): Int {
        val nodesInRange = mutableListOf<Int>()
        
        fun collectInRange(node: TreeNode?) {
            if (node == null) return
            
            when {
                node.`val` < low -> collectInRange(node.right)
                node.`val` > high -> collectInRange(node.left)
                else -> {
                    nodesInRange.add(node.`val`)
                    collectInRange(node.left)
                    collectInRange(node.right)
                }
            }
        }
        
        collectInRange(root)
        return nodesInRange.sum()
    }
```

