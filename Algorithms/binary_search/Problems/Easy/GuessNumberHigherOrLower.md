# Guess Number Higher or Lower
[Leetcode Problem](https://leetcode.com/problems/guess-number-higher-or-lower/description/)

## Problem Description
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API int guess(int num), which returns three possible results:
- -1: Your guess is higher than the number I picked (i.e. num > pick).
- 1: Your guess is lower than the number I picked (i.e. num < pick).
- 0: your guess is equal to the number I picked (i.e. num == pick).

Return the number that I picked.

## Examples

```kotlin
Input: n = 10, pick = 6
Output: 6

Input: n = 1, pick = 1
Output: 1

Input: n = 2, pick = 1
Output: 1

```

## Constraints
- 1 <= n <= 2^31 - 1
- 1 <= pick <= n

## Key Insights
- API-based binary search instead of array comparison
- Three-way comparison using guess() function
- Range search from 1 to n

## Solution Approach
- Search range: 1 to n
- Use guess(mid) instead of direct comparison
- Interpret API response: -1 (go left), 1 (go right), 0 (found)
- Adjust search boundaries based on response

## Complexity Analysis
- Time: O(log n) - Binary search on range 1 to n
- Space: O(1) - Only using primitive variables

## Solution

```kotlin
fun guessNumber(n: Int): Int {
    var start = 1
    var end = n
    
    while (start <= end) {
        val mid = (start + end) / 2
        val result = guess(mid)
        
        when {
            result == 0 -> return mid
            result == -1 -> end = mid - 1
            result == 1 -> start = mid + 1
        }
    }
    
    return -1
}

```
