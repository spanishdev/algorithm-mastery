# Two Pointers

Two Pointers is an efficient technique that uses two pointers to traverse data structures, typically arrays or linked lists, to solve problems in O(n) time that would otherwise require O(n²) brute force approaches.

## When to Use Two Pointers

### Clear Signals
- Sorted array problems
- Find pair/triplet with target sum
- Remove duplicates in-place
- Reverse or palindrome checking
- Merge two sorted arrays
- Partition problems

### Problem Keywords
- "Find two numbers that add up to..."
- "Remove duplicates from sorted array"
- "Reverse array/string in-place"
- "Merge two sorted arrays"
- "Three sum problem"
- "Container with most water"

## Core Template

### Opposite Direction (Start-End)
```kotlin
fun twoPointersOpposite(arr: IntArray, target: Int): Boolean {
    var left = 0
    var right = arr.size - 1
    
    while (left < right) {
        val sum = arr[left] + arr[right]
        when {
            sum == target -> return true
            sum < target -> left++
            sum > target -> right--
        }
    }
    
    return false
}
```

### Same Direction (Fast-Slow)
```kotlin
fun twoPointersSameDirection(arr: IntArray): Int {
    var slow = 0
    
    for (fast in arr.indices) {
        if (shouldKeep(arr[fast])) {
            arr[slow] = arr[fast]
            slow++
        }
    }
    
    return slow  // New length
}
```

### Merge Pattern
```kotlin
fun mergeTwoArrays(arr1: IntArray, arr2: IntArray): IntArray {
    val result = IntArray(arr1.size + arr2.size)
    var i = 0  // arr1 pointer
    var j = 0  // arr2 pointer
    var k = 0  // result pointer
    
    while (i < arr1.size && j < arr2.size) {
        if (arr1[i] <= arr2[j]) {
            result[k++] = arr1[i++]
        } else {
            result[k++] = arr2[j++]
        }
    }
    
    // Copy remaining elements
    while (i < arr1.size) result[k++] = arr1[i++]
    while (j < arr2.size) result[k++] = arr2[j++]
    
    return result
}
```

## Complexity Analysis

| **Aspect** | **Two pointers** | **Brute force** |
|:-----------|:-------------:|:----------------|
| **Time** | O(n) | O(n²) |
| **Space** | O(1) | O(1) to O(n) |
| **Passes** | Single pass | Multiple passes |
| **Efficiency** | Linear scan | Nested loops |

## Problem Categories

### Easy Problems
- Two Sum II (Sorted Array)
- Remove Duplicates from Sorted Array
- Reverse String
- Valid Palindrome
- Merge Sorted Array

### Medium Problems
- 3Sum
- Container With Most Water
- Remove Duplicates from Sorted Array II
- Sort Colors (Dutch Flag)
- Trapping Rain Water

## Common Variations

### Pointer Movement Patterns
```kotlin
// Opposite direction - both pointers move toward center
while (left < right) {
    // Process arr[left] and arr[right]
    left++    // or right--
    right--   // or left++
}

// Same direction - fast pointer leads, slow follows
for (fast in arr.indices) {
    if (condition) {
        arr[slow] = arr[fast]
        slow++
    }
}

// Independent movement - pointers move at different speeds
while (i < arr1.size && j < arr2.size) {
    if (arr1[i] <= arr2[j]) i++ else j++
}
```

### Decision Logic
```kotlin
// Sum-based decisions (sorted arrays)
when {
    sum == target -> return result
    sum < target -> moveLeftPointer()
    sum > target -> moveRightPointer()
}

// Comparison-based decisions (merging)
if (arr1[i] <= arr2[j]) {
    useElementFromArr1()
} else {
    useElementFromArr2()
}

// Condition-based decisions (filtering)
if (shouldKeep(element)) {
    keepElement()
    moveSlowPointer()
}
```

## ⚠️ Common Pitfalls

| **Pitfall** | **Wrong** | **Correct** |
|:------------|:----------|:------------|
| **Boundary Check** | `while (left <= right)` for pairs | `while (left < right)` |
| **Infinite Loop** | Not moving pointers in all cases | Always move at least one pointer |
| **Index Out of Bounds** | `arr[right]` without checking | Check `right < arr.size` |
| **Duplicate Handling** | Count duplicates incorrectly | Skip duplicates with `while` loops |

## Implementation Tips
- Always ensure at least one pointer moves to avoid infinite loops
- Use meaningful variable names (left/right, slow/fast, i/j)
- Handle edge cases (empty arrays, single elements)
- For sorted arrays, think about which direction to move pointers
- Test with arrays of different sizes (0, 1, 2, 3+ elements)

## Advanced Applications
- Cycle Detection - Floyd's algorithm with fast/slow pointers
- Multiple Sum Problems - 3Sum, 4Sum using nested two pointers
- Sliding Window - Can be viewed as same-direction two pointers
- Partitioning - Dutch National Flag, Quick Sort partitioning
- String Problems - Palindrome checking, string reversal

