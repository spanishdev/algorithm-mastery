# Validate Binary Search Tree
[Leetcode Problem](https://leetcode.com/problems/validate-binary-search-tree/description/)

## Problem Description
Given the root of a binary tree, determine if it is a valid binary search tree (BST).
A valid BST is defined as follows:
- The left subtree of a node contains only nodes with keys less than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees must also be binary search trees.

## Examples

```kotlin
Example 1:
Input: root = [2,1,3]
    2
   / \
  1   3
Output: true

Example 2:
Input: root = [5,1,4,null,null,3,6]
    5
   / \
  1   4
     / \
    3   6
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.

```

## Constraints
- The number of nodes in the tree is in the range [1, 10^4].
- -2^31 <= Node.val <= 2^31 - 1

## Key Insights
- Not just parent-child comparison: Must validate against valid range for each node
- Range-based validation: Each node must be within [min, max] bounds
- Recursive validation: Left subtree gets upper bound, right subtree gets lower bound
- Edge cases: Handle null nodes and integer overflow

## Solution Approach
- Use DFS with min/max bounds for each node
- For each node, check if value is within valid range [min, max]
- Recursively validate left subtree with updated upper bound
- Recursively validate right subtree with updated lower bound
- Use Long.MIN_VALUE and Long.MAX_VALUE to handle edge cases

## Complexity Analysis
- Time Complexity: O(n) where n is the number of nodes in the tree
- Space Complexity: O(h) where h is the height of the tree (recursion stack)

## Solution

```kotlin
/**
 * Definition for a binary tree node.
 */
class TreeNode(var `val`: Int) {
    var left: TreeNode? = null
    var right: TreeNode? = null
}

// Optimal Solution - Range Validation O(n)
fun isValidBST(root: TreeNode?): Boolean {
    return isValid(root, Long.MIN_VALUE, Long.MAX_VALUE)
}

fun isValid(node: TreeNode?, min: Long, max: Long): Boolean {
    if(node == null) return true
    val value = node.`val`.toLong()
    
    if(value <= min || value >= max) return false
    
    return isValid(node.left, min, value) && isValid(node.right, value, max)
}

```
