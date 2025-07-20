# Sliding Window
Sliding Window is a powerful technique for solving array/string problems that involve contiguous subarrays by maintaining a dynamic window that expands and contracts, optimizing O(n²) brute force solutions to O(n).

## When to Use Sliding Window
- Contiguous subarray/substring problems
- Find maximum/minimum in subarrays
- "At most K distinct" or "exactly K" conditions
- Sum/average of fixed-size windows
- Pattern matching in strings

## Problem Keywords
- "Find the longest substring..."
- "Maximum sum of subarray of size K"
- "Minimum window substring"
- "At most K distinct characters"
- "All anagrams in string"

## Core Templates

### Fixed Size Window
```kotlin
fun fixedWindow(arr: IntArray, k: Int): Int {
    var windowSum = 0
    var maxSum = 0
    
    // Initialize first window
    for (i in 0 until k) {
        windowSum += arr[i]
    }
    maxSum = windowSum
    
    // Slide the window
    for (i in k until arr.size) {
        windowSum += arr[i] - arr[i - k]  // Add new, remove old
        maxSum = maxOf(maxSum, windowSum)
    }
    
    return maxSum
}
```
### Variable Size Window
```kotlin
fun variableWindow(arr: IntArray, target: Int): Int {
    var start = 0
    var maxLength = 0
    var windowSum = 0
    
    for (end in arr.indices) {
        // Expand window
        windowSum += arr[end]
        
        // Contract window if needed
        while (windowSum > target && start <= end) {
            windowSum -= arr[start]
            start++
        }
        
        // Update result
        if (windowSum == target) {
            maxLength = maxOf(maxLength, end - start + 1)
        }
    }
    
    return maxLength
}
```

## Complexity Analysis
| **Aspect** | **Sliding Window** | **Brute Force** |
|:-----------|:-----------------:|:---------------:|
| **Time** | O(n) | O(n²) or O(n³) |
| **Space** | O(1) to O(k) | O(1) |
| **Optimization** | Single pass | Multiple passes |


## Problem Categories
### Easy Problems
- Maximum Sum Subarray of Size K
- Average of All Subarrays of Size K
- Smallest Subarray with Sum ≥ K

### Medium Problems
- Longest Substring with At Most K Distinct Characters
- Minimum Window Substring
- Longest Substring Without Repeating Characters
- Permutation in String

## Common Variations

### Window Types

```kotlin
// Fixed Size - window size never changes
for (i in k until arr.size) {
    windowSum += arr[i] - arr[i - k]
}

// Variable Size - window expands and contracts
for (end in arr.indices) {
    // Expand
    while (condition) {
        // Contract
        start++
    }
}
```

### Data Structure Usage

```kotlin
// Basic tracking with variables
var windowSum = 0
var maxCount = 0

// Character frequency tracking
val charCount = mutableMapOf<Char, Int>()

// Uniqueness tracking
val charSet = mutableSetOf<Char>()
```

## ⚠️ Common Pitfalls
| **Pitfall** | **Wrong** | **Correct** |
|:------------|:----------|:------------|
| **Window Bounds** | `end - start` | `end - start + 1` |
| **Contract Condition** | `while (invalid)` without bounds | `while (invalid && start <= end)` |
| **State Update** | Update after window change | Update during window change |
| **Edge Cases** | No empty array check | `if (arr.isEmpty()) return 0` |

## Implementation Tips
- Use clear variable names (start/end for pointers, windowSum for tracking)
- Update window state immediately when adding/removing elements
- Handle edge cases (empty arrays, k larger than array size)
- Test with simple examples first (arrays of size 1-3)

## Advanced Applications
- Character Frequency Problems - Use HashMap for tracking
- Multiple Constraints - Combine window techniques with other data structures
- String Pattern Matching - Variable window with complex validity conditions
- Subarray Optimization - Convert nested loops to single pass algorithms
