# Longest Substring with At Most K Distinct Characters
[Leetcode Problem](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

## Problem Description
Given a string `s` and an integer `k`, return the length of the longest substring that contains at most `k` distinct characters.

## Examples
```kotlin
Input: s = "eceba", k = 2
Output: 3
Explanation: "ece" has 2 distinct characters (e, c)

Input: s = "aa", k = 1
Output: 2
Explanation: "aa" has 1 distinct character (a)

Input: s = "abcdef", k = 3
Output: 3
Explanation: "abc" has 3 distinct characters

Input: s = "a", k = 0
Output: 0
Explanation: No substring can have 0 distinct characters
```

## Constraints
- 1 <= s.length <= 5 * 10^4
- 0 <= k <= 50
- s consists of English lowercase letters only

## Key Insights
- Variable window size - expand until constraint violated, then contract
- HashMap tracking - count frequency of each character in current window
- "At most K" means window can have 0, 1, 2, ..., up to K distinct characters
- Contract when > K distinct characters (not when == K)
- Always check for contraction after adding new character

## Solution Approach
Variable-size sliding window with HashMap:

- Expand window by adding characters from right pointer
- Track character frequencies using HashMap
- Contract window from left when distinct characters > K
- Remove characters from HashMap when count reaches 0
- Update maximum length continuously during expansion

## Complexity Analysis
- Time: O(n) - Each character processed at most twice (once by each pointer)
- Space: O(k) - HashMap stores at most k+1 distinct characters during contraction

## Solution
```kotlin
fun lengthOfLongestSubstringKDistinct(s: String, k: Int): Int {
    var max = 0
    var start = 0
    val map = HashMap<Char, Int>()
    
    for (end in s.indices) {
        val char = s[end]
        map[char] = map.getOrDefault(char, 0) + 1
        
        while (map.size > k) {
            val startChar = s[start]
            map[startChar] = map.getOrDefault(startChar, 0) - 1
            if (map[startChar] == 0) map.remove(startChar)
            start++
        }
        
        max = maxOf(max, end - start + 1)
    }
    
    return max
}
```
