# Longest Repeating Character Replacement
[Leetcode Problem](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

## Problem Description
You are given a string s and an integer k.
You can choose any character of the string and change it to any other uppercase English character.
You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

## Examples

```kotlin
Example 1:
Input: s = "ABAB", k = 2
Output: 4
Explanation: Replace the two 'A's with two 'B's or vice versa.

Example 2:
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
There are other ways to achieve this answer too.
```

## Constraints
- 1 <= s.length <= 10^5
- s consists of only uppercase English letters.
- 0 <= k <= s.length

## Key Insights
- Use sliding window with character frequency tracking
- Window is valid if: windowSize - maxFreqChar <= k
- Only need to track the maximum frequency seen so far
- When window becomes invalid, shrink from left
- The key insight: we can change at most k characters to make them all the same

## Solution Approach
- Use a sliding window from start to end.
- Track the frequency of each character within the window using a HashMap.
- Track the maximum frequency (maxFreq) of any character in the current window.
- If the current window size minus maxFreq is greater than k, it means we need to replace more than k characters, so we shrink the window from the left.
- Always update the result with the maximum valid window length.

## Complexity Analysis
- Time Complexity: O(n) where n is the length of string s
- Space Complexity: O(1) since we only store at most 26 characters

## Solution

```kotlin
fun characterReplacement(s: String, k: Int): Int {
    var start = 0
    var maxFreq = 0
    var result = 0
    val frequencies = HashMap<Char, Int>()
    
    for (end in s.indices) {
        val char = s[end]
        frequencies[char] = frequencies.getOrDefault(char, 0) + 1
        maxFreq = maxOf(maxFreq, frequencies[char]!!)
        
        while ((end - start + 1) - maxFreq > k) {
            val startChar = s[start]
            frequencies[startChar] = frequencies[startChar]!! - 1
            start++
        }
        
        result = maxOf(result, end - start + 1)
    }
    
    return result
}

```
