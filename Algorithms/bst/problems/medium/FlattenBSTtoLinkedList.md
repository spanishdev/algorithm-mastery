# Flatten Binary Tree to Linked List
[Leetcode Problem](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/description/)

## Problem Description
Given the root of a binary tree, flatten the tree into a "linked list":
- The "linked list" should use the same TreeNode class where the right child pointer points to the next node in the list and the left child pointer is always null.
- The "linked list" should be in the same order as a pre-order traversal of the binary tree.


## Examples

![flaten](https://github.com/user-attachments/assets/5942fb59-c845-4755-87bd-1085ba874ecd)

```kotlin
Example 1:

Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]

Example 2:

Input: root = []
Output: []

Example 3:

Input: root = [0]
Output: [0]


```

## Constraints
- The number of nodes in the tree is in the range [0, 2000].
- -100 <= Node.val <= 100

## Key Insights

### Critical Understanding:
- Result should follow preorder traversal order (root → left → right)
- Transform tree in-place (modify existing nodes, don't create new ones)
- Final structure: right-skewed tree with all left pointers = null

### Main Challenges:
- Preserve traversal order while modifying structure
- In-place transformation without losing node references
- Pointer manipulation to rewire connections correctly

## Solution Approach
### Approach 1: Preorder Traversal + Rewire (Simplest)
- Collect all nodes in preorder
- Rewire them into right-skewed structure

### Approach 2: Recursive with Previous Node Tracking
- Flatten left and right subtrees recursively
- Connect current node to flattened left subtree
- Connect end of left subtree to flattened right subtree

### Approach 3: Iterative with Stack
- Use stack to simulate preorder traversal
- Rewire connections as we process nodes

### Approach 4: Morris-like In-place (Most Elegant)
- For each node, connect rightmost node of left subtree to right subtree
- Move left subtree to right position

## Complexity Analysis
### Preorder + Rewire:
- Time: O(n) - single traversal + rewiring
- Space: O(n) - store all nodes in list

### Recursive Approaches:
- Time: O(n) - visit each node once
- Space: O(h) - recursion stack depth

### Iterative with Stack:
- Time: O(n) - each node processed once
- Space: O(h) - explicit stack

### Morris-like In-place:
- Time: O(n) - each node visited at most twice
- Space: O(1) - true in-place solution

## Solution

```kotlin
/**
 * Definition for a binary tree node.
 */
class TreeNode(var `val`: Int) {
    var left: TreeNode? = null
    var right: TreeNode? = null
}
```

### Approach 1: Preorder Traversal + Rewire (Clearest)

```kotlin
 fun flatten(root: TreeNode?): Unit {
        if (root == null) return
        
        val preorderNodes = mutableListOf<TreeNode>()
        preorderTraversal(root, preorderNodes)
        
        // Rewire nodes into right-skewed structure
        for (i in 0 until preorderNodes.size - 1) {
            preorderNodes[i].left = null
            preorderNodes[i].right = preorderNodes[i + 1]
        }
        
        // Last node
        preorderNodes.last().left = null
        preorderNodes.last().right = null
    }

    private fun preorderTraversal(node: TreeNode?, nodes: MutableList<TreeNode>) {
        if (node == null) return
        
        nodes.add(node)
        preorderTraversal(node.left, nodes)
        preorderTraversal(node.right, nodes)
    }

```

### Approach 2: Recursive with Tail Tracking (Elegant)

```kotlin
fun flatten(root: TreeNode?): Unit {
    root?.let { node ->
        flatten(node.left)          // Flatten left subtree
        flatten(node.right)         // Flatten right subtree

        val leftFlattened = node.left
        val rightFlattened = node.right

        node.left = null
        node.right = leftFlattened   // Move left to right

        var current = node
        while(current.right != null) { // Find tail of left subtree
            current = current.right
        }

        current.right = rightFlattened // Connect right subtree to tail
    }
}
```


### Approach 3: Iterative with Stack

```kotlin
 if (root == null) return
        
        val stack = mutableListOf<TreeNode>()
        stack.add(root)
        
        while (stack.isNotEmpty()) {
            val current = stack.removeAt(stack.size - 1)
            
            // Add right child first (stack is LIFO)
            current.right?.let { stack.add(it) }
            current.left?.let { stack.add(it) }
            
            // Connect to next node in preorder (top of stack)
            if (stack.isNotEmpty()) {
                current.right = stack.last()
            }
            
            current.left = null
        }
```


### Approach 4: Morris-like In-place (Most Efficient)

#### How it works

```
// Original:     1
//              / \
//             2   5
//            / \   \
//           3   4   6

// Step 1: current = 1, left = 2
// Find rightmost in left subtree = 4
// Connect 4.right = 5 (original right of 1)
//     1
//      \
//       2
//      / \
//     3   4
//          \
//           5
//            \
//             6

// Step 2: Move left to right, clear left
//   1
//    \
//     2
//    / \
//   3   4->5->6

// Continue iteratively...
// Final: 1->2->3->4->5->6
```

#### Implementation
```kotlin
 fun flattenMorris(root: TreeNode?): Unit {
        var current = root
        
        while (current != null) {
            if (current.left != null) {
                // Find the rightmost node in left subtree
                var rightmost = current.left
                while (rightmost?.right != null) {
                    rightmost = rightmost.right
                }
                
                // Connect rightmost to current's right subtree
                rightmost?.right = current.right
                
                // Move left subtree to right
                current.right = current.left
                current.left = null
            }
            
            current = current.right
        }
    }
```
