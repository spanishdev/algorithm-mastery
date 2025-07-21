# Binary Tree Level Order Traversal
[Leetcode Problem](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

## Problem Description
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

## Examples

```kotlin
Example 1:
Input: root = [3,9,20,null,null,15,7]
    3
   / \
  9  20
    /  \
   15   7
Output: [[3],[9,20],[15,7]]

Example 2:
Input: root = [1]
Output: [[1]]

Example 3:
Input: root = []
Output: []

```

## Constraints
- The number of nodes in the tree is in the range [0, 2000].
- -1000 <= Node.val <= 1000

## Key Insights
- This is a classic BFS (Breadth-First Search) problem on trees
- Process nodes level by level using a queue
- Keep track of current level size to group nodes by level
- Each level becomes a separate list in the result

## Solution Approach
- Use a queue to store nodes to be processed
- Start by adding root to queue
- For each level, determine how many nodes are in current level
- Process exactly that many nodes and add their children to queue
- Collect all nodes at current level into a list
- Continue until queue is empty

## Complexity Analysis
- Time Complexity: O(n) where n is the number of nodes in the tree
- Space Complexity: O(w) where w is the maximum width of the tree (queue size)

## Solution DFS

```kotlin
/**
 * Definition for a binary tree node.
 */
class TreeNode(var `val`: Int) {
    var left: TreeNode? = null
    var right: TreeNode? = null
}

fun levelOrderDFS(root: TreeNode?): List<List<Int>> {
    if(root == null) return emptyList()
    
    val result = mutableListOf<MutableList<Int>>()
    levelList(root, 0, result)
    
    return result as List<List<Int>>
}

fun levelList(node: TreeNode?, level: Int, list: MutableList<MutableList<Int>>) {
    if(node == null) return
    
    if(list.size <= level) {
        list.add(mutableListOf<Int>()) 
    }
    list[level].add(node.`val`)
    
    levelList(node.left, level + 1, list)
    levelList(node.right, level + 1, list)
}

```

## Solution BFS

```kotlin
/**
 * Definition for a binary tree node.
 */
class TreeNode(var `val`: Int) {
    var left: TreeNode? = null
    var right: TreeNode? = null
}

fun levelOrder(root: TreeNode?): List<List<Int>> {
    if(root == null) return emptyList()
    
    val result = mutableListOf<MutableList<Int>>()
    val queue = ArrayDeque<TreeNode>()
    queue.add(root)
    
    while(queue.isNotEmpty()) {
        val levelSize = queue.size
        val currentLevel = mutableListOf<Int>()
        
        for(i in 0 until levelSize) {
            val node = queue.removeFirst()
            currentLevel.add(node.`val`)
            
            node.left?.let { queue.add(it) }
            node.right?.let { queue.add(it) }
        }
        
        result.add(currentLevel)
    }
    
    return result as List<List<Int>>
}

```
