# Priority Queue (Heap) - Complete Reference

## Heap Overview
A Heap is a complete binary tree represented as an array where:

- Min Heap: Each parent node is smaller than or equal to its children.
- Max Heap: Each parent node is greater than or equal to its children.
- The tree is filled level by level, left to right.
- Provides efficient access to the minimum (Min Heap) or maximum (Max Heap) element.

## Visual representation

```
// For a heap: [3, 5, 8, 10, 15, 12]

       3
      / \
     5   8
    / \   \
   10 15   12

```

### Navigation Formulas
- Parent of index i: (i - 1) / 2
- Left child of i: 2 * i + 1
- Right child of i: 2 * i + 2

### Comparison Abstraction
```kotlin
private fun T?.compare(other: T): Boolean = 
  this != null && when (type) {
    HeapType.MIN -> this < other
    HeapType.MAX -> this > other
  }
```

## API Summary

| **Property**    | **Type**  | **Description**          |
| --------------- | --------- | ------------------------ |
| `size`          | `Int`     | Number of elements       |
| `isEmpty`       | `Boolean` | True if empty            |
| `peek()`        | `T?`      | Returns root element     |
| `insert(value)` | `Unit`    | Inserts an element       |
| `extract()`     | `T?`      | Removes and returns root |

## Class Structure

### Heap type enum
```kotlin
enum class HeapType { MIN, MAX }
```
Defines whether the heap maintains minimum or maximum at the root.

### Insertion

```kotlin
val minHeap = Heap<Int>(HeapType.MIN)
minHeap.insert(10)
minHeap.insert(5)
minHeap.insert(15)
```
- Adds element to the end of the array.
- Bubbles up (heapifyUp) until heap property is restored.

### Extraction

```kotlin
val min = minHeap.extract() // Returns smallest element
```
#### Extraction Process

- Save root element (result to return)
- Move last element to root position
- Remove last element from array
- Bubble down (heapifyDown) from root until heap property is restored


## Heap types

### Min Heap

```kotlin
val minHeap = Heap<Int>(HeapType.MIN)
minHeap.insert(10)
minHeap.insert(5)
minHeap.insert(20)

// Heap: [5, 10, 20]
// Extract order: 5, 10, 20 (ascending)
```

### Max Heap

```kotlin
val maxHeap = Heap<Int>(HeapType.MAX)
maxHeap.insert(10)
maxHeap.insert(5)
maxHeap.insert(20)

// Heap: [20, 5, 10]
// Extract order: 20, 10, 5 (descending)
```
### Complexity analysis

| **Operation** | **Time Complexity** | **Space Complexity** |
| ------------- | ------------------- | -------------------- |
| **Peek**      | O(1)                | O(1)                 |
| **Insert**    | O(log n)            | O(1)                 |
| **Extract**   | O(log n)            | O(1)                 |
| **Space**     | -                   | O(n)                 |

## Full Implementation

```kotlin
enum class HeapType { MIN, MAX }

class Heap<T: Comparable<T>>(private val type: HeapType = HeapType.MIN) {
    private val list: MutableList<T> = mutableListOf()
    
    val size get() = list.size 
    val isEmpty get() = list.isEmpty()
    
    private fun Int.leftChild() = 2 * this + 1
    private fun Int.rightChild() = 2 * this + 2
    private fun Int.parent() = (this - 1) / 2
    
    fun peek() = list.firstOrNull()
    
    fun insert(value: T) {
        list.add(value)    
        heapifyUp()
    }
    
    fun extract(): T? {
        if(isEmpty) return null 
        val item = list[0]
        heapifyDown()
        return item
    }
    
    private fun heapifyUp() {
        if(list.isEmpty()) return
        
        var currentIndex = size - 1
        
        while(currentIndex > 0) {
            val parentIndex = currentIndex.parent()
            val current = list[currentIndex]
            val parent = list[parentIndex]
            
            if(!current.compare(parent)) return
            
            swap(currentIndex, parentIndex)
            currentIndex = parentIndex
        }
    }
    
    private fun heapifyDown() {
        if(size <= 1) return
        
        swap(0, size - 1)
        list.removeLast()
        
        var currentIndex = 0
        
        while(currentIndex < size) {
            val leftIndex = currentIndex.leftChild()
            val rightIndex = currentIndex.rightChild()
            
            var bestIndex = currentIndex
            
            if(list.getOrNull(leftIndex)?.compare(list[bestIndex]) == true) bestIndex = leftIndex
            if(list.getOrNull(rightIndex)?.compare(list[bestIndex]) == true) bestIndex = rightIndex
            
            if(currentIndex == bestIndex) return
            
            swap(currentIndex, bestIndex)
            currentIndex = bestIndex
        }
    }
    
    private fun T?.compare(other: T) = this != null && when(type) {
        HeapType.MIN -> this < other
        HeapType.MAX -> this > other
    }
    
    private fun swap(i: Int, j: Int) {
        if(i < 0 || i >= size || j < 0 || j >= size) return 
        
        val aux = list[i]
        list[i] = list[j]
        list[j] = aux
    }
}
```

## Using Kotlin's PriorityQueue

| **Custom Heap** | **PriorityQueue** | **Description** |
| insert()        | add() / offer()   | Insert element  |
| extract()       | poll() / remove() | Extract root    |
| peek()          | peek()            | View root       |

### Basic usage

```kotlin
import java.util.PriorityQueue

// Min Heap (default)
val minHeap = PriorityQueue<Int>()
minHeap.add(10)
minHeap.add(5)
minHeap.add(20)
println(minHeap.poll()) // 5

// Max Heap using reverseOrder()
val maxHeap = PriorityQueue<Int>(reverseOrder())
maxHeap.addAll(listOf(10, 5, 20))
println(maxHeap.poll()) // 20
```

### Custom Comparators

```kotlin
// Custom comparator for max heap
val maxHeap = PriorityQueue<Int> { a, b -> b.compareTo(a) }

// Objects with custom comparison
data class Task(val priority: Int, val name: String)

// Min heap by priority
val taskQueue = PriorityQueue<Task>(compareBy { it.priority })

// Max heap by priority, then by name
val complexQueue = PriorityQueue<Task>(
    compareByDescending<Task> { it.priority }.thenBy { it.name }
)

// Multiple criteria
val heap = PriorityQueue<Person>(
    compareBy<Person> { it.age }.thenBy { it.name }
)
```


