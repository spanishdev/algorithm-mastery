# Combinations
[Leetcode Problem](https://leetcode.com/problems/combinations/description/)

## Problem Description
Given two integers n and k, return all possible combinations of k numbers chosen from the range [1, n].
You may return the answer in any order.

## Examples

```kotlin
Example 1:

Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.

Example 2:

Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination.
```

## Constraints
- 1 <= n <= 20
- 1 <= k <= n

## Key Insights

### Problem Analysis:
- Generate all possible combinations (not permutations!)
- Order doesn't matter: [1,2] same as [2,1]
- No repetition: each number used at most once
- Choose exactly k numbers from range [1,n]

### Backtracking Pattern:
- Choose a number to include
- Explore combinations with that choice
- Unchoose (backtrack) and try next number

### Key Observations:
- Combinations avoid duplicates by only going forward (no [2,1] after [1,2])
- Use "start" parameter to ensure we only pick numbers >= current
- Base case: when combination has k elements


## Templating for Backtrackin

```kotlin
fun backtrack(start, currentCombination) {
    if (currentCombination.size == k) {
        result.add(currentCombination.copy())  // Found valid combination
        return
    }
    
    for (i in start..n) {
        currentCombination.add(i)           // Choose
        backtrack(i + 1, currentCombination) // Explore
        currentCombination.removeAt(size-1)  // Unchoose (backtrack)
    }
}

```

## Problem variants

## Variant 1: Standard Combinations (Original Problem)
No repetition: each number used at most once
Order doesn't matter: [1,2] same as [2,1]
Result: [1,2], [1,3], [1,4], [2,3], [2,4], [3,4]

## Variant 2: Combinations with Repetition
Allow same number multiple times: [1,1] valid
Still no order: [1,2] same as [2,1]
Result: [1,1], [1,2], [1,3], [1,4], [2,2], [2,3], [2,4], [3,3], [3,4], [4,4]

## Variant 3: All Possible Paths (All Permutations)
All numbers available at each position
Order matters: [1,2] ≠ [2,1]
Result: [1,1], [1,2], [1,3], [1,4], [2,1], [2,2], [2,3], [2,4], [3,1], [3,2], [3,3], [3,4], [4,1], [4,2], [4,3], [4,4]


## Complexity Analysis

### Standard Combinations:
Time: O(C(n,k) × k) where C(n,k) = n!/(k!(n-k)!)
Space: O(C(n,k) × k) for storing all combinations + O(k) recursion depth

### Combinations with Repetition:
Time: O(C(n+k-1,k) × k) - stars and bars formula
Space: O(C(n+k-1,k) × k) for storing results

### All Possible Paths:
Time: O(n^k × k) - n choices for each of k positions
Space: O(n^k × k) for storing all paths


## Solutions

### Approach 1: Backtracking Combination without repetition

Example: We have N numbers in combinations wiht K size.
With N = 4 and K = 2

[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]

```kotlin
 fun combine(n: Int, k: Int): List<List<Int>> {
        val list: MutableList<List<Int>> = mutableListOf()
        combine(1, n, k, mutableListOf(), list)
        return list
    }

    fun combine(start: Int, end: Int, k: Int, path: MutableList<Int>, result: MutableList<List<Int>>) {
        if(path.size == k) {
            result.add(path.toList()) //Important to add a copy, or the list will mess up
            return
        }

        for(num in start..end) {
            path.add(num)
            combine(num + 1, end, k, path, result) //num + 1 avoids repetition
            path.removeLast()
        }
    }

```

### Approach 2: Backtracking Combination with repetition

Example: We have N numbers in combinations wiht K size.
With N = 4 and K = 2

[1,1], [1,2], [1,3], [1,4], [2,2], [2,3], [2,4], [3,3], [3,4]

```kotlin
 fun combine(n: Int, k: Int): List<List<Int>> {
        val list: MutableList<List<Int>> = mutableListOf()
        combine(1, n, k, mutableListOf(), list)
        return list
    }

    fun combine(start: Int, end: Int, k: Int, path: MutableList<Int>, result: MutableList<List<Int>>) {
        if(path.size == k) {
            result.add(path.toList()) //Important to add a copy, or the list will mess up
            return
        }

        for(num in start..end) {
            path.add(num)
            combine(num, end, k, path, result) //Now we start from num to add duplicated nums
            path.removeLast()
        }
    }

```

### Approach 3: Backtracking Permutations without repetition

Example: 
[1,2,3] → [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```kotlin
class PermutationsSolution {
    // Your style adapted for permutations without repetition
    fun permute(nums: IntArray): List<List<Int>> {
        val result: MutableList<List<Int>> = mutableListOf()
        val used = BooleanArray(nums.size)
        permute(nums, used, mutableListOf(), result)
        return result
    }
    
    fun permute(
        nums: IntArray,
        used: BooleanArray, 
        path: MutableList<Int>, 
        result: MutableList<List<Int>>
    ) {
        if (path.size == nums.size) {
            result.add(path.toList()) // Important to add a copy, or the list will mess up
            return
        }
        
        for (i in nums.indices) {
            if (!used[i]) {
                used[i] = true
                path.add(nums[i])
                permute(nums, used, path, result)
                path.removeLast()
                used[i] = false
            }
        }
    }
}

```

### Approach 4: Backtracking Permutations with repetition

Example: 
n=3, k=2 → [[1,1],[1,2],[1,3],[2,1],[2,2],[2,3],[3,1],[3,2],[3,3]]

```kotlin
class PermutationsWithRepetitionSolution {
    // Your exact style for permutations with repetition
    fun permuteWithRep(n: Int, k: Int): List<List<Int>> {
        val result: MutableList<List<Int>> = mutableListOf()
        permuteWithRep(0, n, k, mutableListOf(), result)
        return result
    }
    
    fun permuteWithRep(depth: Int, n: Int, k: Int, path: MutableList<Int>, result: MutableList<List<Int>>) {
        if (depth == k) {
            result.add(path.toList()) // Important to add a copy, or the list will mess up
            return
        }
        
        for (num in 1..n) {  // Always 1..n for all positions
            path.add(num)
            permuteWithRep(depth + 1, n, k, path, result) // depth tracks position, not start
            path.removeLast()
        }
    }
}

```

