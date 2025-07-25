# Binary Search

Binary Search is a fundamental algorithm that efficiently searches for elements in **sorted data structures** by repeatedly dividing the search space in half.

## When to Use Binary Search

### Clear Signals
- **O(log n) time complexity requirement**
- **Sorted array or data structure**
- **Search space can be divided** systematically
- **Large datasets** where linear search is too slow

### Problem Keywords
- "Find target in sorted array"
- "Search in O(log n) time"
- "Find first/last occurrence"
- "Search in rotated sorted array"

## Core Template

```kotlin
fun binarySearch(nums: IntArray, target: Int): Int {
    var start = 0
    var end = nums.size - 1
    
    while (start <= end) {
        val mid = (start + end) / 2
        when {
            nums[mid] == target -> return mid
            nums[mid] > target -> end = mid - 1
            nums[mid] < target -> start = mid + 1
        }
    }
    
    return -1  // Not found
}
```

## Complexity Analysis

| **Aspect** | **Complexity** | **Explanation** |
|:-----------|:-------------:|:----------------|
| **Time** | O(log n) | Eliminates half of search space each iteration |
| **Space** | O(1) | Only uses constant extra variables |
| **Best Case** | O(1) | Target found at middle position |
| **Worst Case** | O(log n) | Target at boundary or not found |

## Problem Categories

### Easy Problems

### Medium Problems
- [Search a 2D Matrix](./Problems/Medium/Search2dMatrix_1.md)
- [Find First and Last Position](./Problems/Medium/FindFirstLastElemSortedArray.md)
- [Find Peak Element](./Problems/Medium/FindPeakElement.md)

## Common Variations

### Boundary Conditions
```kotlin
// Standard: start <= end (searches until convergence)
while (start <= end) { ... }

// Modified: start < end (stops when adjacent)
while (start < end) { ... }
```

### Index Updates
```kotlin
// Standard updates
start = mid + 1  // Search right half
end = mid - 1    // Search left half

// Modified for boundary finding
end = mid        // Include mid in next iteration
start = mid + 1  // Exclude mid from next iteration
```

## ⚠️ Common Pitfalls

| **Pitfall** | **Wrong** | **Correct** |
|:------------|:----------|:------------|
| **Infinite Loop** | `start = mid` | `start = mid + 1` |
| **Boundary Direction** | `nums[mid] > target → start = mid + 1` | `nums[mid] > target → end = mid - 1` |
| **Index Bounds** | `mid + 1` without checking | Check `mid < nums.size - 1` |
| **Empty Array** | No check | `if (nums.isEmpty()) return -1` |

## Implementation Tips
- Always handle edge cases (empty arrays, single elements)
- Use clear variable names (start/end vs left/right)
- Consistent boundary updates (mid ± 1)
- Test with small arrays first

## Advanced Applications
- 2D Matrix Search - Treat as flattened 1D array
- Rotated Arrays - Modified comparison logic
- Peak Finding - Gradient-based elimination
- Range Queries - Multiple coordinated searches

