# Binary Search Tree - Complete Reference

## BST Overview
A Binary Search Tree (BST) is a node-based binary tree where:

- The left subtree of a node contains values less than the node’s key.
- The right subtree contains values greater than the node’s key.
- Duplicate keys are not allowed (by design in this implementation).

## Class Structure

### TreeNode
```kotlin
class TreeNode<T: Comparable<T>>(var `val`: T) {
    var left: TreeNode<T>? = null
    var right: TreeNode<T>? = null
}
```
Each node stores a value and references to its left and right child nodes.

### BST API Summary

| **Function**  | **Signature**                                    | **Description**                                            |
| ------------- | ------------------------------------------------ | ---------------------------------------------------------- |
| **Insert**    | `fun insert(value: T)`                           | Inserts a new value                                        |
| **Find**      | `fun find(value: T): TreeNode<T>?`               | Returns node with given value (or null)                    |
| **Delete**    | `fun delete(value: T, strategy: DeleteStrategy)` | Deletes a node using `SUCCESSOR` or `PREDECESSOR` strategy |
| **Preorder**  | `fun preorder(): List<T>`                        | Root → Left → Right                                        |
| **Inorder**   | `fun inorder(): List<T>`                         | Left → Root → Right                                        |
| **Postorder** | `fun postorder(): List<T>`                       | Left → Right → Root                                        |
| **Count**     | `val count: Int`                                 | Number of nodes in the tree                                |

### Insertion

```kotlin
val bst = BST<Int>()
bst.insert(10)
bst.insert(5)
bst.insert(15)
``` 
- Recursively finds correct position and inserts node.
- Increments count only when a new node is created.

### Deletion

#### Strategy Enum

```kotlin
enum class DeleteStrategy {
    SUCCESSOR, PREDECESSOR
}
```

#### Deletion Logic 

| Case         | Behavior                                       |
| ------------ | ---------------------------------------------- |
| Leaf         | Deletes node directly                          |
| One child    | Bypasses node, links child                     |
| Two children | Replaces with inorder successor or predecessor |

```kotlin
bst.delete(10, DeleteStrategy.SUCCESSOR)
bst.delete(5, DeleteStrategy.PREDECESSOR)
```

### Tree Traversals

#### Preorder (Root → Left → Right)

```kotlin
val result = bst.preorder()

   10
  /  \
 5    15

Output: [10, 5, 15]
```

#### Inorder (Left → Root → Right)

```kotlin
val result = bst.inorder()

   10
  /  \
 5    15

Output: [5, 10, 15]
```

#### Postorder (Left → Right → Root)

```kotlin
val result = bst.postorder()

   10
  /  \
 5    15

Output: [5, 15, 10]
```

## Full Implementation

```kotlin
/**
 * Definition for a binary tree node.
 */
class TreeNode<T: Comparable<T>>(var `val`: T) {
    var left: TreeNode<T>? = null
    var right: TreeNode<T>? = null
}

enum class DeleteStrategy {
	SUCCESSOR, PREDECESSOR,
}

class BST<T: Comparable<T>> {

	private var root: TreeNode<T>? = null
	var count: Int = 0
		private set
		
	
	// Find DST
	fun find(value: T): TreeNode<T>? {
		return findRecursive(value, root)
	}
	
	private fun findRecursive(value: T, node: TreeNode<T>?): TreeNode<T>? {
		if(node == null) return null
		
		return when {
			value < node.`val`  -> findRecursive(value, node.left)
			value > node.`val` -> findRecursive(value, node.right)
			else -> node
		}
	}
		
	fun insert(value: T) {
		if(root == null) {
			count++
			root = TreeNode(value)
		} else {
			insertRecursive(value, root)
		}
	}
	
	private fun insertRecursive(value: T, node: TreeNode<T>) {
		when {
			value < node.`val` ->
				if(node.left != null) insertRecursive(value, node.left!!)
				else {
					node.left = TreeNode(value)
					count++
				}
				
			value > node.`val` ->
				if(node.right != null) insertRecursive(value, node.right!!)
				else {
					node.right = TreeNode(value)
					count++
				}
				
			 else -> return
		}
	}
	
	fun delete(value: T, strategy: DeleteStrategy) {
		root = deleteRecursive(value, root, strategy)
	}
	
	private fun deleteRecursive(value: T, node: TreeNode<T>?, strategy: DeleteStrategy): TreeNode<T>? {
		if(node == null) return	 null
		
		return when {
			value < node.`val` -> {
				node.left = deleteRecursive(value, node.left, strategy)
				node
			}
			value > node.`val` -> {
				node.right = deleteRecursive(value, node.right, strategy)
				node
			}
			else -> handleDeletion(value, node, strategy)
		}
	}
	
	private fun handleDeletion(value: T, node: TreeNode<T>?, strategy: DeleteStrategy): TreeNode<T>? {
		return when {
			node.left == null && node.right == null -> {
				count--
				null
			}
			node.left == null -> {
				count--
				node.right 
			}
			node.right == null -> {
				count--
				node.left 
			}
			else -> when(strategy) {
				DeleteStrategy.SUCCESSOR -> {
					val minNode = findMin(node.right!!)
					node.`val` = minNode.`val`
					node.right = deleteRecursive(minNode.`val`, node.right, strategy)
					node
				}
				DeleteStrategy.PREDECESSOR -> {
					val maxNode = findMax(node.left!!)
					node.`val` = maxNode.`val`
					node.left = deleteRecursive(maxNode.`val`, node.left, strategy)
					node
				}
			}
		}
	}
	
	private fun findMin(node: TreeNode<T>): TreeNode<T> {
		var current = node 
		while(current.left != null) {
			current = current.left
		}
		
		return current
	}
	
	private fun findMax(node: TreeNode<T>): TreeNode<T> {
		var current = node 
		while(current.right != null) {
			current = current.right
		}
		
		return current
	}
	
	fun preorder(): List<T> {
		val list = mutableListOf<T>()
		preorderRecursive(root, list)
		return list.toList()
	}
	
	private fun preorderRecursive(current: TreeNode<T>?, list: MutableList<T>) {
		if(current == null) return 
		
		list.add(current.`val`)
		preorderRecursive(current.left, list)
		preorderRecursive(current.right, list)
	}
	
	fun inorder(): List<T> {
		val list = mutableListOf<T>()
		inorderRecursive(root, list)
		return list.toList()
	}
	
	private fun inorderRecursive(current: TreeNode<T>?, list: MutableList<T>) {
		if(current == null) return 
		
		inorderRecursive(current.left, list)
		list.add(current.`val`)
		inorderRecursive(current.right, list)
	}
	
		
	fun postorder(): List<T> {
		val list = mutableListOf<T>()
		postorderRecursive(root, list)
		return list.toList()
	}
	
	private fun postorderRecursive(current: TreeNode<T>?, list: MutableList<T>) {
		if(current == null) return 
		
		postorderRecursive(current.left, list)
		postorderRecursive(current.right, list)
		list.add(current.`val`)
	}
	
}
```


